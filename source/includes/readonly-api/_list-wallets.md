# Read-only API code API
## GET / List Wallets

``` shell

curl http://localhost:8889/v1/mock/wallets/readonly/walletlist

# An example of a successful response:
{
  "total": 2,
  "wallets": [
    {
      "address": "2NAnSkEp6SpUPLsdP3ChvN6K5qPMZyoB3RF",
      "currency": 0,
      "currency_name": "BTC",
      "decimals": "8",
      "type": 2,
      "wallet_id": 101645
    },
    {
      "address": "0x85AfD8F88C0347aFF89AFc6C0749322719396616",
      "currency": 60,
      "currency_name": "ETH",
      "decimals": "18",
      "token_contract_address": "0xdf2ce4af00b10644d00316b3d99e029d82d5d2f3",
      "token_decimals": "18",
      "token_name": "JGB2",
      "token_symbol": "JGB2",
      "type": 0,
      "wallet_id": 118970
    }
  ]
}

```

List all wallets can be accessed by the inquiry read-only API code.


### Request
 **GET** /v1/sofa/wallets/readonly/walletlist

<aside class="notice">
 The API code must be a read-only API code.
</aside>


---

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| address | string | Wallet address |
| currency | int64 | Registered coin types. Refer to [Currency Definition](#currency-definition) |
| currency_name | string | Name of currency |
| decimals | string | Decimals of currency |
| type | int | Wallet Type. Refer to [Wallet Type Definition](#wallet-type-definition)|
| wallet_id | int64 | Wallet ID |
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
