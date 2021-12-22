## GET / Query Vault Wallet Balance

``` shell

curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/vault/balance

# An example of a successful response:
# BTC vault wallet
{
  "balance": "0.00009798",
  "currency": 0,
  "token_address": "",
  "token_balance": "",
  "unconfirm_balance": "0",
  "unconfirm_token_balance": "",
  "wallet_address": "2Mw1iJnQvAt3hNEvEZKdHkij8TNtzjaF3LH"
}

# USDT-Omni vault wallet that mapping to above BTC vault wallet
{
  "balance": "0.00009798",
  "currency": 0,
  "token_address": "31",
  "token_balance": "0.1",
  "unconfirm_balance": "0",
  "unconfirm_token_balance": "",
  "wallet_address": "2Mw1iJnQvAt3hNEvEZKdHkij8TNtzjaF3LH"
}

```

Get the vault wallet balance. Facilitate to establish a real-time balance monitoring mechanism.

### Request

**GET** /v1/sofa/wallets/`WALLET_ID`/vault/balance

<aside class="notice">
 WALLET_ID must be a withdrawal wallet ID
</aside>

---

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| currency | int64 | Registered coin types. Refer to [Currency Definition](#currency-definition) |
| wallet_address | string | Wallet address |
| token_address | string | Token contract address |
| balance | string | Withdrawal wallet balance |
| token_balance | string | Withdrawal wallet token balance |
| unconfirm\_balance | string | Unconfirmed withdrawal wallet balance |
| unconfirm\_token_balance | string | Unconfirmed withdrawal wallet token balance |

### The currencies that support the unconfirmed balance are BTC, LTC, ETH, BCH, BSV, DASH

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
| 404 | 304 | Wallet ID invalid | - | The wallet is not allowed to perform this request |
| 403 | 385   | API Secret not valid | - | Invalid API code permission |
