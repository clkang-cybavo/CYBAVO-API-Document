## GET / Query Deposit Wallet Balance

``` shell

curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/receiver/balance

# An example of a successful response:
{
  "currency": 60,
  "token_address": "",
  "balance": "0.619673333517576",
  "token_balance": ""
}

```

Get the deposit wallet balance.


##### Request
**GET** /v1/sofa/wallets/`WALLET_ID`/receiver/balance

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| currency | int64 | Registered coin types. Refer to [Currency Definition](#currency-definition) |
| token_address | string | Token contract address |
| balance | string | Deposit wallet balance |
| token_balance | string | Deposit wallet token balance |

##### Error Code

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
| 404 | 304 | Wallet ID invalid | - | The wallet is not allowed to perform this request |
