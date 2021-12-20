## POST / Resend Deposit/Collection Callbacks

``` shell

curl -X POST -H "Content-Type: application/json" -d '{"notification_id": 0}' \
 'http://localhost:8889/v1/sofa/wallets/179654/collection/notifications/manual'

# An example of a successful response:
{
  "count": 0
}
```

The callback handler can call this API to resend pending, risk-controlled or failed deposit/collection callbacks.

Refer to [Callback Integration](#callback-integration) for callback rules.

- The resend operation could be requested on the web control panel as well.


##### Request
**POST** /v1/sofa/wallets/`WALLET_ID`/collection/notifications/manual

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>


---

The request includes the following parameters:

###### Post body

| Field | Type  | Note | Description |
| :---  | :---  | :---  | :---        |
| notification_id | int64 | required, 0 means all | Specify callback ID to resend |

### This ID equal to the serial field of callback data.

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| count | int | Count of callbacks just resent |


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
| 400 | 112 | Invalid parameter | - | Malformatted post body |
| 404 | 304 | Wallet ID invalid | - | The wallet is not allowed to perform this request |
