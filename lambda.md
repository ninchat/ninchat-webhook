# Ninchat webhooks for AWS Lambda

Webhooks are delivered by invoking a [Lambda](https://aws.amazon.com/lambda/)
function with a JSON document.  If the function returns an error, it will be
retried later.

Ninchat AWS account `691075289290` must be granted the `lambda:InvokeFunction`
permission for the webhook handler function.


## JSON document

Top-level properties which are only present when using the
[wrapped](webhook.md#wrapped) format:

- `kid` string identifies the signing key.

- `exp` number is the expiration time of the request as a Unix timestamp.

Top-level properties which are always present:

- `aud` string indicates the target/owner of the webhook.  The string can be
  obtained from Ninchat while configuring webhooks: its scope can be realm,
  realm owner (user) or company.  It must always be verified before processing
  a request.

- `event` string is the event name.

- `event_id` string is a unique identifier for the event instance.  If it's
  seen again, it means that a previous request is being retried (i.e. the
  connection failed before Ninchat could receive the response).  The webhook
  processor should ignore the duplicate event if possible, and respond with a
  status code indicating success.

Event type dependent top-level properties:

- There is a property named after the event.  Its type depends on the event.

