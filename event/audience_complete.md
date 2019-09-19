# Ninchat webhook event: `audience_complete`

The webhook document contains the `event_id` and `audience_complete`
properties.  The latter is an object with the following properties:

- `realm_id` string.
- `queue_id` string.
- `audience_id` string.
- `audience` object.
- `channel_id` string (optional).
- `dialogue_id` string array (optional).
- `messages` object array (optional).

The webhook contains `channel_id`, `dialogue_id` or neither.  If neither is
present, `messages` is not present either.

The `audience` object contains the following properties:

- `request_time` number.
- `accept_time` number (optional).
- `finish_time` number (optional).
- `complete_time` number.
- `members` object containing user ids.  Each user id is mapped to an object which may contain either the `agent` or the `customer` boolean property.
- `metadata` object which contains audience metadata.

`accept_time` and `finish_time` are available only for channel/dialogue
audiences.

The `messages` objects contain the following properties:

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
    "event":    "audience_complete",
    "event_id": "38hj5ip6789gf",
    "audience_complete": {
        "realm_id":    "abc123",
        "queue_id":    "def456",
        "audience_id": "38hj5ip6789ge",
        "audience": {
            "request_time":  1445592871.785086,
            "accept_time":   1445592877.183851,
            "finish_time":   1445592952.232474,
            "complete_time": 1445592955.341256,
            "members": {
                "05kq2htc":      {"agent": true},
                "38hj5ip5000eg": {"customer": true}
            },
            "metadata": {
                "MessageOfTheDay": "Get to the chopper!",
                "secure": {
                    "Customer number": "123-456"
                }
            }
        },
        "channel_id": "01234abc",
        "channel": {
            "metadata": {
                "OnTheFly": "stuff"
            }
        },
        "messages": [
            {
                "id":        "0fb74jl5",
                "time":      1320846070.265,
                "type":      "ninchat.com/text",
                "user_id":   "05kq2htc",
                "user_name": "Pekka",
                "fold":      true,
                "payload":   {"text": "Hello, Pirjo!"}
            },
            {
                "id":        "0fb74jl6",
                "time":      1320846071.376,
                "type":      "ninchat.com/metadata",
                "user_id":   "05kq2htc",
                "user_name": "Pekka",
                "payload":   {"data": {"OnTheFly": "stuff"}}
            }
        ]
    }
}
```

```
HTTP/1.1 200 OK
Content-Length: 0
```
