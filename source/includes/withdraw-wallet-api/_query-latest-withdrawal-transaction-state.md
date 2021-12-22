## GET / Query Latest Withdrawal Transaction State

``` shell

# The prefix is 888888_ in following sample request.

curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/transactions/{ORDER_ID}

# An example of a successful response:
{
  "order_id": "888888_1",
  "address": "0xaa0cA2f9bA3A33a915a27e289C9719adB2ad7d73",
  "amount": "1.11",
  "memo": "",
  "in_chain_block": 1016603,
  "txid": "db0f3a27de564a411aeff1d2cb3234c54817de1ecc2258a510a50c5a1063d41c",
  "create_time": "2020-03-16T10:27:57Z"
}
```

Check the latest withdrawal transaction state of certain order ID.

### Request
**GET** /v1/sofa/wallets/`WALLET_ID`/sender/transactions/`ORDER_ID`

<aside class="notice">
 WALLET_ID must be a withdrawal wallet ID
</aside>

### The order ID is used in the [withdraw assets](#post-withdraw-assets) API.

---

### Response Format

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
| 404 | 304 | Wallet ID invalid | - | The {ORDER\_ID} not found |
