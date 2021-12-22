## GET / Query Wallet Info

``` shell

curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/info

# An example of a successful response:
{
  "currency": 60,
  "currency_name": "ETH",
  "address": "0xd11Bd6E308b8DC1c5243D54cf41A427Ca0F46943",
  "token_name": "TTF TOKEN",
  "token_symbol": "TTF",
  "token_contract_address": "0xd0ee17a4e1866c1ac53a54cc2cd4dd64b503cf40",
  "token_decimals": "18"
}

```

Get wallet basic information.

### Request
 **GET** /v1/sofa/wallets/`WALLET_ID`/info

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>


---

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| currency | int64 | Registered coin types. Refer to [Currency Definition](#currency-definition) |
| currency_name | string | Name of currency |
| address | string | Wallet address |

### Refer to [here](https://github.com/satoshilabs/slips/blob/master/slip-0044.md) for more detailed currency definitions

If `WALLET_ID` is a token wallet, the following fields present:

| Field | Type  | Description |
| :---  | :---  | :---        |
| token_name | string | Token name |
| token_symbol | string | Token symbol |
| token\_contract_address | string | Token contract address |
| token_decimals | string | Token decimals |


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
