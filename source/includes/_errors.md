# Errors

<aside class="notice">
The API will generally <strong>always</strong> respond with an <code>HTTP Status 200</code>.
</aside>

If an error occurs, the `errors` property will be available in the response.

```json
{
    "errors": {
      "SIGNATURE_MISMATCH": "The request signature provided does not match the requested resource signature"
    }
}
```

## App Errors

Error Code | Meaning
--- | ---
`SIGNATURE_REQUIRED` | The required `signature` parameter is missing from the request.
`SIGNATURE_MISMATCH` | The `signature` provided does not match the `signature` required for the requested resource.
`RELATIONSHIP_MISSING` | The requested `relationship` does not exist for the given resource.

## Server Errors

Error Code | Meaning
--- | ---
`429` | Rate Limit Error -- You sent too many requests. Please look at the <a href="#rate-limiting">Rate Limiting Section</a>.
`500` | Internal Server Error -- We had a problem with our server. Please try again later.
`503` | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
