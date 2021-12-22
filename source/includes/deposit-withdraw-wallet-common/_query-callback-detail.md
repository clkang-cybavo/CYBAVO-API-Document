## POST / Query Callback Detail

``` shell
# Post body
{
  "ids": [
    90000000140,
    90000000139    
  ]
}

curl -X POST -H "Content-Type: application/json" -d '{"ids":[90000000140,90000000139]}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/notifications/get_by_id

# An example of a successful response:
{
  "notifications": [
    {
      "type": 3,
      "serial": 90000000139,
      "order_id": "",
      "currency": "ADA",
      "txid": "35c283a6f13f5886240fe2e815bc149154ec066cd2061202318dd4e4bf8af35e",
      "block_height": 1003304,
      "tindex": 0,
      "vout_index": 0,
      "amount": "24447",
      "fees": "0",
      "memo": "",
      "broadcast_at": 1584088556,
      "chain_at": 1584088556,
      "from_address": "",
      "to_address": "37btjrVyb4KG8gKeZjJguinwdsbcRV65ngHhBUaJWf36QxiakTV3UHiNUP9arReXMZQnpRBVVdkcBB4GyiWzPRSTmg41mTzMpxgfhtfRHtaBCKJNbX",
      "wallet_id": 677414,
      "state": 3,
      "confirm_blocks": 2,
      "processing_state": 1,
      "addon": {}
    },
    {
      "type": 3,
      "serial": 90000000140,
      "order_id": "",
      "currency": "ADA",
      "txid": "fa120b6283509f0ab2b136a3ac8b613aa3ca2f36ce7c2744e122668d013cfdb5",
      "block_height": 1003305,
      "tindex": 0,
      "vout_index": 0,
      "amount": "55497180",
      "fees": "0",
      "memo": "",
      "broadcast_at": 1584088576,
      "chain_at": 1584088576,
      "from_address": "",
      "to_address": "37btjrVyb4KDKCyAPRUPxpGiUPWunpBAkGRX8U3h7LYzS2UrHUnEQozcCyqR2GfBVnM3frTaUNEb8DoNGo9JakrskAtaWt6vED6R6ohkmaJ2qr4oCg",
      "wallet_id": 677414,
      "state": 3,
      "confirm_blocks": 1,
      "processing_state": 1,
      "addon": {}
    }
  ]
}

```

Query the detailed information of the callback by its serial ID. It can be used to reconfirm whether a deposit callback exists.

### Request
**POST** /v1/sofa/wallets/`WALLET_ID`/notifications/get\_by_id

<aside class="notice">
 WALLET_ID must be a withdrawal wallet ID
</aside>

---

### Post body

| Field | Type | Note | Description |
| :---  | :--- | :--- | :---        |
| ids | array | requried | Specify the IDs for query |

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| notifications | array | Arrary of callbacks, refer to [Callback Definition](#callback-definition) |

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
