# Ninchat webhook event: `audience_accepted`

The webhook document contains the `audience_accepted` property.  It is an
object with the following properties:

- `realm_id` string.
- `queue_id` string.
- `audience_id` string.
- `audience` object.
- `channel_id` string (optional).
- `dialogue_id` string array (optional).

The webhook contains `channel_id` or `dialogue_id`.

The `audience` object contains the following properties:

- `request_time` number.
- `accept_time` number.
- `members` object containing user ids.  Each user id is mapped to an object which may contain either the `agent` or the `customer` boolean property.
- `metadata` object which contains audience metadata.


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
    "event":    "audience_accepted",
    "event_id": "38hj5ip6789gf",
    "audience_accepted": {
        "realm_id":    "abc123",
        "queue_id":    "def456",
        "audience_id": "38hj5ip6789ge",
        "audience": {
            "request_time":  1445592871.785086,
            "accept_time":   1445592877.183851,
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
        "channel_id": "01234abc"
    }
}
```

```
HTTP/1.1 200 OK
Content-Length: 0
```
