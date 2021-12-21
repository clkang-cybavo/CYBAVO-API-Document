## GET / Query Withdrawal Whitelist

``` shell

curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/whitelist

# An example of a successful response:
{
  "items": [
    {
      "address": "GCIFMEYIEWSX3K6EOPMEJ3FHW5AAPD6NW76J7LPBRAKD4JZKTISKUPHJ",
      "create_time": "2020-12-30T06:02:25Z",
      "effective": false,
      "effective_time": "2020-12-30T13:27:37Z",
      "memo": "",
      "state": 1,
      "update_time": "2020-12-30T06:02:25Z",
      "user_id": "USER001"
    },
  ],
  "total_count": 1
}

```

Used to query some kind of callbacks within a time interval.

### Request
**GET** /v1/sofa/wallets/`WALLET_ID`/sender/whitelist?from\_time=`from`&to\_time=`to`&start\_index=`offset`&request_number=`count`&state=`state`

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>

---

### Query Parameters

| Field | Type  | Note | Description |
| :---  | :---  | :--- | :---        |
| from_time | int64 | optional, default `0` | Start date (unix time in UTC) |
| to_time | int64 | optional, default `current time` | End date (unix time in UTC) |
| start_index | int64 | optional, default `0` | The offset to the first entry |
| request_number | int64 | optional, default `1000`, max `2000` | The count to request |
| state | int | optional, default `-1` | Use `1` to query the active entries and `2` to query the removed entries, `-1` means all entries |

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| items | array | Arrary of the whitelist entries |
| address | string | The whitelist entry address |
| create_time | string | The creation time in UTC |
| effective | boolean | Indicate whether the whitelist entry has taken effect |
| effective_time | string | The effective time in UTC |
| memo | string | The memo of the whitelist entry |
| state | int | `1` means the entry is active, `2` means the entry is removed |
| update_time | string | Last modification time in UTC |
| user_id | string | The custom user ID of the whitelist entry |

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
| 404 | 304 | Wallet ID invalid | - | The wallet is not allowed to perform this request |
