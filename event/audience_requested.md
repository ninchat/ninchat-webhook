# Ninchat webhook event: `audience_requested`

The webhook document contains the `event_id` and `audience_requested`
properties.  The latter is an object with the following properties:

- `realm_id` string.
- `queue_id` string.
- `queue_members` object array (optional).
- `audience_id` string.
- `audience` object.

The `queue_members` array (if any) contains objects which are described in the
[audience_accepted](audience_accepted.md) document.

The `audience` object contains the following properties:

- `request_time` number.
- `members` object containing a user id.  The user id is mapped to an object which contains the `customer` boolean property.
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
    "event":    "audience_requested",
    "event_id": "38hj5ip6789gf",
    "audience_requested": {
        "realm_id": "abc123",
        "queue_id": "def456",
        "queue_members": [
            {
                "user_id": "16ir3iud"
            },
            {
                "user_id": "05kq2htc",
                "identifiers": {
                    "urn:oid:1.2.840.113549.1.9.1": ["jane.user@example.com"],
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": ["4066c3507538"]
                }
            }
        ],
        "audience_id": "38hj5ip6789ge",
        "audience": {
            "request_time": 1445592871.785086,
            "members": {
                "38hj5ip5000eg": {"customer": true}
            },
            "metadata": {
                "MessageOfTheDay": "Get to the chopper!",
                "secure": {
                    "Customer number": "123-456"
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


## Notes

### Direct accept URL

The `queue_id` and `audience_id` values from the webhook can be used to form a
URL for accepting the audience:

    https://ninchat.com/app/#/x/accept/QUEUE_ID/AUDIENCE_ID

The specified audience is accepted automatically when the URL is opened in a
browser.

