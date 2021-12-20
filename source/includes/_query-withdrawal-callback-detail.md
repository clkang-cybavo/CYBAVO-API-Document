## GET / Query Withdrawal Callback Detail

``` shell

curl 'http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/notifications/order_id/{ORDER_ID}'

# An example of a successful response:
{
  "notification": {
    "addon": {},
    "amount": "100000000000000",
    "block_height": 7813953,
    "broadcast_at": 1588211126,
    "chain_at": 1588211126,
    "confirm_blocks": 109490,
    "currency": "ETH",
    "fees": "21000000000000",
    "from_address": "0xaa0cA2f9bA3A33a915a27e289C9719adB2ad7d73",
    "memo": "",
    "order_id": "888888_1",
    "processing_state": 2,
    "serial": 90000000554,
    "state": 3,
    "tindex": 30,
    "to_address": "0x60589A749AAC632e9A830c8aBE042D1899d8Dd15",
    "txid": "0x471c11f139ce1a7e0627a05cea50d64e47e797c94fd72025f1978cc919e07aa9",
    "type": 2,
    "vout_index": 0,
    "wallet_id": 68451
  }
}

```

Query the detailed information of the withdrawal callback by the order ID.

### This API only provides in-chain transactions query, for those not in-chain transactions use [Query All Withdrawal Transaction States](#get-query-all-withdrawal-transaction-states) API instead.

### Request
 **GET** /v1/sofa/wallets/`WALLET_ID`/sender/notifications/order_id/`ORDER_ID`

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
| 404 | 304 | Wallet ID invalid | - | The wallet is not allowed to perform this request or the {ORDER\_ID} is not found |