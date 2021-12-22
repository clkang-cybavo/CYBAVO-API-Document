## POST / Query Transaction Average Fee

``` shell

# Post body
{
  "block_num": 1
}

curl -X POST -H "Content-Type: application/json" -d '{"block_num":1}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/autofee

# An example of a successful response:
{
	"auto_fee": "1"
}

```

Query average blockchain fee within latest N blocks.


### Request
**POST** /v1/sofa/wallets/`WALLET_ID`/autofee

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>

---

### Post body

| Field | Type  | Note | Description |
| :---  | :---  | :---     | :---        |
| block_num | int | optional, default `1`, range `1~100` | Query the average blockchain fee in the last N blocks |


### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| auto_fee | string | Mining fee denominated in the smallest cryptocurrency unit |

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
| 400 | 112 | Invalid parameter | - | The `block_num` is out of range |
