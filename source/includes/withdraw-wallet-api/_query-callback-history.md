## GET / Query Callback History

``` shell

curl 'http://localhost:8889/v1/mock/wallets/{WALLET_ID}/notifications?from_time=1561651200&to_time=1562255999&type=2'

# An example of a successful response:
{
  "notifications": [
    {
      "type": 2,
      "serial": 90000000003,
      "order_id": "a206",
      "currency": "BNB",
      "txid": "76B8B2B1E25472FFE7B8FCE85742E0964FEDB1B679DE963FA19F430E8B287F93",
      "block_height": 25844472,
      "tindex": 2,
      "vout_index": 0,
      "amount": "15000000",
      "fees": "37500",
      "memo": "CC",
      "broadcast_at": 0,
      "chain_at": 1562234190,
      "from_address": "tbnb1f805kv6z8nq2whrcnkagjte3jjss2sxf2rfls0",
      "to_address": "tbnb1655kasahedvaeudaeq6jggr7kal8qgwygu9xqk",
      "wallet_id": 677414,
      "state": 3,
      "confirm_blocks": 1,
      "processing_state": 1,
      "addon": {}
    }
  ]
}

```

Used to query some kind of callbacks within a time interval.

### Request
**GET** /v1/sofa/wallets/`WALLET_ID`/notifications?from\_time=`from`&to\_time=`to`&type=`type`

<aside class="notice">
 WALLET_ID must be a withdrawal wallet ID
</aside>

---

### Query Parameters

| Field | Type  | Note | Description |
| :---  | :---  | :--- | :---        |
| from_time | int64 | optional, default `0` | Start date (unix time in UTC) |
| to_time | int64 | optional, default `current time` | End date (unix time in UTC) |
| type | int | optional, default `-1` | Refer to [Callback Type](#callback-type-definition) |

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
