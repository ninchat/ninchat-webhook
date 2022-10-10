# Ninchat webhook event: `audience_agent_released`

The webhook document contains the `event_id` and `audience_agent_released`
properties.  The latter is an object with the following properties:

- `realm_id` string.
- `queue_id` string.
- `queue_member` object.
- `audience_id` string.
- `audience` object.
- `channel_id` string.

The `queue_member` object identifies the agent user.  See
[audience_accepted](audience_accepted.md) for its description.
