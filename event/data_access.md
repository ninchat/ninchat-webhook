# Ninchat webhook event: `data_access`

The webhook document contains the `data_access` property.  It is an object with
the following properties:

- `realm_id` string.
- `channel_id` string (optional).
- `query` object.

The webhook currently contains `channel_id`, but more options may be added in
the future.

The `query` object contains the following properties:

- `audience.metadata` boolean (optional).
- `channel.metadata` boolean (optional).
- `messages` object (optional).

The `messages` objects contain the following properties:

- `min_id` string (optional).
- `max_id` string (optional).


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
    "kid":   "ninchat.com/ed25519-2019-02",
    "exp":   1445593256,
    "aud":   "realm:abc123",
    "event": "data_access",
    "data_access": {
        "realm_id":   "abc123",
        "channel_id": "01234abc",
        "query": {
            "audience.metadata": true,
            "channel.metadata":  true,
            "messages": {
                "min_id": "abc123",
                "max_id": "456def"
            }
        }
    }
}
```

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 1357
```

```json
{
    "audience": {
        "metadata": {
            "MessageOfTheDay": "Get to the chopper!",
            "secure": {
                "Customer number": "123-456"
            }
        }
    },
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
```


### Example code

Ninchat Go [webhook package](https://godoc.org/github.com/ninchat/ninchat-go/webhook)
includes [an example](https://github.com/ninchat/ninchat-go/blob/master/webhook/example/processor.go)
of storing data via [`audience_complete`](audience_complete.md) and loading it
using `data_access`.

