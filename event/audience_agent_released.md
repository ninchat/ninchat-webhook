# Ninchat webhook event: `audience_agent_released`

The webhook document contains the `event_id` and `audience_agent_released`
properties.  The latter is an object with the following properties:

- `realm_id` string.
- `realm` object.
- `queue_id` string.
- `queue` object.
- `queue_member` object.
- `audience_id` string.
- `audience` object.
- `channel_id` string.

The `realm` and `queue` objects contain the `name` property.

The `queue_member` object identifies the agent user.  See
[audience_accepted](audience_accepted.md) for its description.


### Example

See the [audience_accepted example](audience_accepted.md#example) (just replace
`audience_accepted` with `audience_agent_released` everywhere).

