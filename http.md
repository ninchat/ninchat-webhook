# Ninchat HTTP webhooks

Webhooks are delivered using HTTP POST requests.  The webhook processor must
support HTTPS with TLS version 1.2.  HTTP response codes 200-204 are considered
successful.  Other codes cause the request to be retried later.

HTTP requests must be authenticated by verifying their
[signatures](webhook.md#signature-verification).  When using the
[default](webhook.md#default) format, the signature is provided in the
`X-Ninchat-Signature` HTTP header.  When using the
[wrapped](webhook.md#wrapped) format, the signature is provided in the request
content.

The `X-Ninchat-Format` HTTP header indicates the delivery format: `default` or
`wrapped`.

The `User-Agent` HTTP header is a string which starts with `ninchat-webhook/`.


## JSON document

Top-level properties:

- `kid` string identifies the signing key.

- `exp` number is the expiration time of the request as a Unix timestamp.
  Expired requests should not be processed as a security precation.  Replay
  attacks can be mitigated by storing each signature string until expiration.

- `aud` string indicates the target/owner of the webhook.  The string can be
  obtained from Ninchat while configuring webhooks: its scope can be realm,
  realm owner (user) or company.  It must always be verified before processing
  a request.

- `event` string is the event name.

Event type dependent top-level properties:

- `event_id` string is a unique identifier for the event instance.  If it's
  seen again, it means that a previous request is being retried (i.e. the
  connection failed before Ninchat could receive the response).  The webhook
  processor should ignore the duplicate event if possible, and respond with a
  status code indicating success.

- There is a property named after the event.  Its type depends on the event.


## Endpoint verification

When webhooks are configured, Ninchat will send a verification request to the
webhook URL.  It looks like a normal webhook, with event type
`webhook_verification`, but without an event id.  The `webhook_verification`
property is a challenge string which must be echoed back: the HTTP response
body must be a JSON document with the `aud` and `webhook_verification`
properties.  The value of `aud` must be `https://ninchat.com`.

If the HTTP response has unexpected status code, content type or contents,
verification will not be retried automatically, and webhooks will not be
activated.  To retry verification, webhook configuration must be reactivated
via Ninchat.

Only response codes 200 and 203 are considered successful for verification
requests.


### Example

```
POST /webhook-handler HTTP/1.1
Host: processor.example.com
User-Agent: ninchat-webhook/1
Date: Tue, 15 May 2018 07:12:31 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 1357
X-Ninchat-Signature: 59bb0e6b7c94c53559f3d1b407831622b34c3f292005c5e7314904860883cf27
```

```json
{
    "kid":                  "ninchat.com/ed25519-2019-02",
    "exp":                  1445591256,
    "aud":                  "realm:6c4e0055",
    "event":                "webhook_verification",
    "webhook_verification": "flkejl4jr3as32"
}
```

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 35
```

```json
{
    "aud":                  "https://ninchat.com",
    "webhook_verification": "flkejl4jr3as32"
}
```

