# Ninchat webhook event: `message_sent`

The webhook document contains the `event_id` and `message_sent` properties.  The
latter is an object with the following properties:

- `realm_id` string.
- `queue_id` string (optional).
- `audience_id` string (optional).
- `channel_id` string.
- `message` object.

The `message` object contain the following properties:

- `id` string which is unique within the audience.
- `time` as a number.
- `type` string is [`ninchat.com/file`](https://ninchat.com/file), [`ninchat.com/info/*`](https://ninchat.com/info), [`ninchat.com/metadata`](https://ninchat.com/metadata), [`ninchat.com/notice`](https://ninchat.com/notice), [`ninchat.com/text`](https://ninchat.com/text) or [`ninchat.com/ui/*`](https://ninchat.com/ui).  More options may be added in the future.
- `user_id` string identifies the author of the message.
- `user_name` string (optional) is the author's name at the time of the message.
- `fold` boolean (optional) means that this message replaces an earlier message with the same type from the same author.
- `payload`, the type of which depends on `type`.  All supported message types can be represented using a single JSON value: this is the deserialized representation.


### Example

```
POST /webhook-handler HTTP/1.1
Host: processor.example.com
User-Agent: ninchat-webhook/1
Date: Tue, 15 May 2018 08:12:31 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 1357
X-Ninchat-Signature: 3f61a902f5c473ef8238bd52b84dbcd3382e0d6c7e0a1605ac85cc79a1e34eec
```

```json
{
    "kid":      "ninchat.com/ed25519-2019-02",
    "exp":      1445593256,
    "aud":      "realm:abc123",
    "event":    "message_sent",
    "event_id": "38hj5ip6789gf",
    "message_sent": {
        "realm_id":    "abc123",
        "queue_id":    "def456",
        "audience_id": "38hj5ip6789ge",
        "channel_id":  "01234abc",
        "message": {
            "id":        "0fb74jl5",
            "time":      1320846070.265,
            "type":      "ninchat.com/text",
            "user_id":   "05kq2htc",
            "user_name": "Pekka",
            "payload":   {"text": "Hello, Pirjo!"}
        }
    }
}
```

```
HTTP/1.1 200 OK
Content-Length: 0
```
