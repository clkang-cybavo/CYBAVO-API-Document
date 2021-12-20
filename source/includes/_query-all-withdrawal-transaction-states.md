## GET / Query All Withdrawal Transaction States

``` shell

# The prefix is 888888_ in following sample request.

curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/transactions/{ORDER_ID}/all

# An example of a successful response:
# The sample shows the states of a resent transaction
{
  "transactions": [
    {
      "address": "0x36a49c68EF1e3f39CDbaE2f5636C74BA10815cea",
      "amount": "0.105",
      "create_time": "2020-09-24T03:43:17Z",
      "in_chain_block": 0,
      "memo": "",
      "order_id": "888888_1",
      "state": 6,
      "txid": "0x2a8a44f1cfed9cd7b86d86170e2418566765f88c5186246f571374df218fd1a1"
    },
    {
      "address": "0x36a49c68EF1e3f39CDbaE2f5636C74BA10815cea",
      "amount": "0.105",
      "create_time": "2020-09-24T03:44:35Z",
      "in_chain_block": 8742982,
      "memo": "",
      "order_id": "888888_1",
      "state": 4,
      "txid": "0xfbeaae4b87f977bcce8ef44672e035d287b96be24e779757c1a7f598501881ef"
    }
  ]
}
```

Check the all withdrawal transaction states of certain order ID.

##### Request
 **GET** /v1/sofa/wallets/`WALLET_ID`/sender/transactions/`ORDER_ID`/all

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>

### The order ID is used in the [withdraw assets](#post-withdraw-assets) API.

---

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| order_id | string | The unique ID specified in `sender/transactions` API |
| address | string | Outgoing address |
| amount | string | Withdrawal amount |
| memo | string | Memo on blockchain |
| in\_chain\_block | int64 | The block that contains this transaction |
| txid | string | Transaction ID |
| create_time | string | The withdrawal time in UTC |
| state | int | Refer to [Transaction State Definition](#transaction-state-definition) |

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
| 404 | 304 | Wallet ID invalid | - | The {ORDER\_ID} not found |
