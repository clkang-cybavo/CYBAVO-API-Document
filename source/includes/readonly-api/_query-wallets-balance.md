## GET / Query Wallets Balance

``` shell

curl http://localhost:8889/v1/mock/wallets/readonly/walletlist/balances

# An example of a successful response:
{
  "total": 104,
  "wallet_balances": [
    {
      "balance": "1",
      "type": 0,
      "wallet_id": 417702
    },
    {
      "balance": "0.35673727953125",
      "token_balance": "0.00000099",
      "type": 0,
      "wallet_id": 426493
    },
    {
      "balance": "4.661838507219943297",
      "token_id_balances": [
        {
          "balance": "4",
          "token_id": "2001"
        },
        {
          "balance": "50000",
          "token_id": "2004"
        },
        {
          "balance": "0",
          "token_id": "2005"
        }
      ],
      "type": 0,
      "wallet_id": 661159
    },
    {
      "balance": "0.010000000000000000",
      "pool_balance": "0.414979",
      "type": 2,
      "wallet_id": 520474
    },
    {
      "balance": "27.46735753510800289",
      "token_balance": "0",
      "type": 3,
      "wallet_id": 100587
    }
  ]
}
```

Query balance of all wallets can be accessed by the inquiry read-only API code.


### Request

 **GET** /v1/sofa/wallets/readonly/walletlist/balances?type=`type`&start_index=`start_index`&request\_number=`request_number`

<aside class="notice">
 The API code must be a read-only API code.
</aside>


---

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| wallet_id | int64 | Wallet ID |
| wallet_name | string | Wallet Name |
| address | string | Wallet address |
| currency | int64 | Registered coin types. Refer to [Currency Definition](#currency-definition) |
| currency_name | string | Name of currency |
| decimals | string | Decimals of currency |
| type | int | Wallet Type. Refer to [Wallet Type Definition](#wallet-type-definition)|
| token_name | string | Token name |
| token_symbol | string | Token symbol |
| token\_contract_address | string | Token contract address |
| token_decimals | string | Token decimals |
| error | boolean | Set to true if the balance query fails |
| balance | string | Wallet balance. For token wallet this is mapping wallet's balance. |
| unconfirm\_balance | string | Unconfirmed wallet balance. For token wallet this is mapping wallet's unconfirmed balance. |
| token_balance | string | Wallet token balance |
| unconfirm\_token_balance | string | Unconfirmed wallet token balance |
| pool_balance | string | Wallet pool address balance (Deposit Wallet only) |
| pool_unconfirm\_token_balance | string | Wallet Pool address unconfirmed balance (Deposit Wallet only) |
| token_id_balances | array | For ERC1155 token wallet |

### Refer to [Support Unconfirmed Balance Currency](#support-unconfirmed-balance-currency) for the currencies that support the unconfirmed balance.

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
