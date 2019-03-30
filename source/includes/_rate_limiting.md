# Rate Limiting

<aside class="notice">
Requests to the EMR4DW API are limited to 60 requests in any 1 minute period. 
</aside>

EMR4DW API responses contain two important headers that should be noted.

The `X-Rate-Limit` header shows you the total number of requests that can be made in any `1 Minute` period.

The `X-RateLimit-Remaining` header shows you the total number of _remaining_ requests that can be made in this `1 Minute` period before a `429 Too Many Requests` response is thrown.

Once a rate limit has been hit, and the server is responding with a `429 Too Many Requests` response, the server will send 2 additional headers.

The `retry-after` header shows you the total number of seconds to wait before sending another request.

The `x-ratelimit-reset` header shows you a timestamp of when you may make another request.

> An example of the rate limiting headers sent by the server

```json
{
    "x-ratelimit-limit": [
      "60"
    ],
    "x-ratelimit-remaining": [
      "0"
    ],
    "retry-after": [
      "31"
    ],
    "x-ratelimit-reset": [
    " 1553985940"
    ]
}
```
