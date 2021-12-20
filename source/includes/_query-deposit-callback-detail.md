## GET / Query Deposit Callback Detail

``` shell

curl 'http://localhost:8889/v1/mock/wallets/{WALLET_ID}/receiver/notifications/txid/{TX_ID}/{VOUT_INDEX}'

# An example of a successful response:
{
  "notification": {
    "addon": {},
    "amount": "2000000000000000000",
    "block_height": 7757485,
    "broadcast_at": 1587441501,
    "chain_at": 1587441501,
    "confirm_blocks": 166027,
    "currency": "ETH",
    "fees": "126000000000000",
    "from_address": "0x8382Cc1B05649AfBe179e341179fa869C2A9862b",
    "memo": "",
    "order_id": "",
    "processing_state": 2,
    "serial": 90000000547,
    "state": 3,
    "tindex": 27,
    "to_address": "0x32d638773cB85965422b3B98e9312Fc9392307BC",
    "txid": "0xb72a81976f780445decd13a35c24974c4e32665cb57d79b3f7a601c775f6a7d8",
    "type": 1,
    "vout_index": 0,
    "wallet_id": 179654
  }
}
```

Query the detailed information of the deposit callback by the tx ID and the vout index.

### Request
**GET** /v1/sofa/wallets/`WALLET_ID`/receiver/notifications/txid/`TX_ID`/`VOUT_INDEX`

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>


---

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| notification | object | Refer to [Callback Definition](#callback-definition) |

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
| 404 | 304 | Wallet ID invalid | - | The wallet is not allowed to perform this request or the callback (txid+vout_index) not found |