# Ninchat webhook event: `audience_request_dropped`

The webhook document contains the `event_id` and `audience_request_dropped`
properties.  The latter is an object with the following properties:

- `realm_id` string.
- `realm` object.
- `queue_id` string.
- `queue` object.
- `audience_id` string.
- `audience` object.

The `realm` and `queue` objects contain the `name` property.

The `audience` object contains the following properties:

- `request_time` number.
- `drop_time` number.


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
    "event":    "audience_request_dropped",
    "event_id": "38hj5ip6789gf",
    "audience_request_dropped": {
        "realm_id": "abc123",
        "queue_id": "def456",
        "audience_id": "38hj5ip6789ge",
        "audience": {
            "request_time": 1445592871.785086,
            "drop_time":    1445593982.896197
        }
    }
}
```

```
HTTP/1.1 200 OK
Content-Length: 0
```
