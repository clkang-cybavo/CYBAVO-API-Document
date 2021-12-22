## POST / Refresh API Code

``` shell

# Post body
{
  "block_num": 1
}

curl -X POST -H "Content-Type: application/json" -d '{"refresh_code":"3EbaSPUpKzHJ9wYgYZqy6W4g43NT365bm9vtTfYhMPra"}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/refreshsecret

# An example of a successful response:
{
  "api_code": "4QjbY3qES4tEh19PU",
  "api_secret": "3jC1qjr4mrKxfoXkxoN27Uhmbm1E",
  "refresh_code": "HcN17gxZ3ojrBYSXnjKsU9Pun8krP6J9Pn678k4rZ13m"
}

```

Use paired refresh code to acquire the new inactive API code/secret of the wallet.


### Request
**POST** /v1/sofa/wallets/`WALLET_ID`/refreshsecret

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>

---

### Post body

| Field | Type  | Note | Description |
| :---  | :---  | :---     | :---        |
| refresh_code | string | required | The corresponding refresh code of the API code specified in the X-API-CODE header |

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| api_code | string | The new inactive API code |
| api_secret | string | The API secret |
| refresh_code | string | The paired refresh code |

### Error Code

| HTTP Code | Error Code | Error | Message | Description |
| :---      | :---       | :---  | :---    | :---        |
| 403 | -   | Forbidden. Invalid ID | - | No wallet ID found |
| 403 | -   | Forbidden. Header not found | - | Missing `X-API-CODE`, `X-CHECKSUM` header or query param `t` |
| 403 | -   | Forbidden. Invalid timestamp | - | The timestamp `t` is not in the valid time range |
| 403 | -   | Forbidden. Invalid checksum | - | The request is considered a replay request |
| 403 | -   | Forbidden. Invalid API code | - | `X-API-CODE` header contains invalid API code |
| 403 | -   | Invalid API code for wallet {WALLET_ID} | - | The API code mismatched |
| 403 | -   | Forbidden. Checksum unmatch | - | `X-CHECKSUM` header contains wrong checksum |
| 403 | -   | Forbidden. Call too frequently ({THROTTLING_COUNT} calls/minute) | - | Send requests too frequently |
| 403 | 385   | API Secret not valid | - | Invalid API code permission |
| 400 | 112 | Invalid parameter | - | Malformatted post body or the refresh code is invalid |
