# Ninchat webhooks

## Event types

- [Audience requested](event/audience_requested.md)
- [Audience request dropped](event/audience_request_dropped.md)
- [Audience accepted](event/audience_accepted.md)
- [Audience agent released](event/audience_agent_released.md)
- [Audience complete](event/audience_complete.md)
- [Message sent](event/message_sent.md)


## Delivery mechanisms

Supported receiver types:

- [HTTP server](http.md)
- [AWS Lambda function](lambda.md)


## Delivery formats

### Default

By default the webhook JSON document is provided directly in the request body.


### Wrapped

Wrapped webhook encapsulates the webhook document into an outer JSON document.
The wrapper contains the following properties:

- `wrapped` boolean with value `true`.  The webhook document doesn't contain
  the `wrapped` property or its value is not `true`, so this can be used to
  detect whether the request content is wrapped or not.

- `signature` string contains a signature of the wrapped content.  It uses
  lower-case hexadecimal encoding.

- `base64` string contains the webhook document in JSON-serialized and
  base64-encoded form.  The decoded form (JSON) is to be used for signature
  verification.


## Signature verification

If the delivery mechanism or format supports it, the content is signed using an
asymmetric cryptographic algorithm.  The signature can be verified using
Ninchat's public key.  The specific key is identified by the webhook document,
and the matching public key data can be retrieved from
https://api.ninchat.com/config/keys.json as a [JSON Web Key
Set](https://tools.ietf.org/html/rfc7517).  Key rotations are communicated in
advance, so the public key can be stored (or at least cached) by the webhook
processor.

Currently only the [Ed25519](https://en.wikipedia.org/wiki/EdDSA) signature
algorithm is supported.

The signature string can also be used to uniquely identify the webhook request;
each delivery attempt of the same event will have a different signature.


## Support library

Ninchat Go package includes [webhook support](https://pkg.go.dev/github.com/ninchat/ninchat-go/webhook).

