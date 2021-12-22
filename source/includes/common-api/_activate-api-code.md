# Common API
## POST / Activate API Code

``` shell

curl -X POST http://localhost:8889/v1/mock/wallets/{WALLET_ID}/apisecret/activate

# An example of a successful response:
{
  "api_code": "4PcdE9VjXfrk7WjC1",
  "exp": 1609646716
}

```

Activate the API code of a certain wallet. Once activated, the currently activated API code will immediately become invalid.


### Request

**POST** /v1/sofa/wallets/`WALLET_ID`/apisecret/activate

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>

---


### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| api_code | string | The activated API code |
| exp | int64 | The API code expiration unix time in UTC |

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
