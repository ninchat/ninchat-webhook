# Ninchat webhook event: `audience_complete`

The webhook document contains the `event_id` and `audience_complete`
properties.  The latter is an object with the following properties:

- `realm_id` string.
- `realm` object.
- `queue_id` string.
- `queue` object.
- `queue_member` object (optional).
- `audience_id` string.
- `audience` object.
- `channel_id` string (optional).
- `dialogue_id` string array (optional).
- `member_message_metadata` object (optional).
- `messages` object array (optional).
- `tagging` object (optional).

The webhook contains `channel_id`, `dialogue_id` or neither.  If neither is
present, `queue_member` and `messages` are not present either.

The `realm` and `queue` objects contain the `name` property.

See [audience_accepted](audience_accepted.md) for a description of the
`queue_member` object.

The `audience` object contains the following properties:

- `request_time` number.
- `accept_time` number (optional).
- `finish_time` number (optional).
- `complete_time` number.
- `members` object containing user ids.  Each user id is mapped to an object which may contain either the `agent` or the `customer` boolean property.
- `metadata` object which contains audience metadata.

`accept_time` and `finish_time` are available only for channel/dialogue
audiences.

The `member_message_metadata` object contains user ids mapped to metadata
objects.  It is a summary of the metadata messages.

The `messages` array contains objects which are described in the
[message_sent](message_sent.md) document.

The `tagging` object contains the following properties:

- `tags` object contains details of the "tag_ids" metadata sent by the acceptor.  Each tag id is mapped to an object which may contain the `name` and `parent_id` properties.
- `parents` object contains details of relevant parent tags.  It is similar to `tags`.


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
        "realm_id": "abc123",
        "queue_id": "def456",
        "queue_member": {
            "user_id": "05kq2htc",
            "identifiers": {
                "urn:oid:1.2.840.113549.1.9.1": ["jane.user@example.com"],
                "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": ["4066c3507538"]
            }
        },
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
        "member_message_metadata": {
            "05kq2htc": {
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
                "payload":   {"text": "Hello, Pirjo!"}
            },
            {
                "id":        "0fb74jl6",
                "time":      1320846071.376,
                "type":      "ninchat.com/metadata",
                "user_id":   "05kq2htc",
                "user_name": "Pekka",
                "fold":      true,
                "payload":   {"data": {"tag_ids": ["34567"]}}
            }
        ],
        "tagging": {
            "tags": {
                "34567": {
                    "name":      "My blue tag",
                    "parent_id": "23456"
                }
            },
            "parents": {
                "12345": {
                    "name": "My tag group"
                },
                "23456": {
                    "name":      "Mid-level"
                    "parent_id": "12345"
                }
            }
        }
    }
}
```

```
HTTP/1.1 200 OK
Content-Length: 0
```
