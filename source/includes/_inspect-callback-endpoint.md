## POST / Inspect Callback Endpoint

``` shell

# Post body
{
  "test_number": 102999
}

curl -X POST -H "Content-Type: application/json" -d '{"test_number":102999}' \
http://localhost:8889/v1/mock/wallets/896541/notifications/inspect

# An example of a successful response:
{
  "server_time": 1622195253,
  "client_ip": "::1",
  "notification_endpoint": {
    "url": "http%3A%2F%2Flocalhost%3A8889%2Fv1%2Fmock%2Fwallets%2Fcallback",
    "status_code": 400,
    "response": "NDAw"
  },
  "withdrawal_authentication_endpoint": {
    "error": "no endpoint found"
  }
}
```

Use to inspect the notification and withdrawal authentication endpoint.

### Request

**POST** /v1/sofa/wallets/`WALLET_ID`/notifications/inspect

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>

---

### Post body

| Field | Type  | Note | Description |
| :---  | :---  | :--- | :---        |
| test_number | int64 | requried | The test number will be sent to the notification endpoint in the format `{"msg":"CONNECTION TEST","server_time":1622195270,"client_ip":"xxx.xxx.xxx.xxx","test_number":102999}`. |

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| server_time | int64 | Current server unix time in UTC |
| client_ip | string | The request client IP |
| notification\_endpoint | struct | Specify the test result of notification endpoint |
| withdrawal\_authentication_endpoint | struct | Specify the test result of withdrawal authentication endpoint|
| url | string | The escaped endpoint URL |
| status_code | int | The HTTP response status code from endpoint |
| response | string | The base64 encoded response from endpoint |
| error | string | Specify the connection error if any |

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
| 400 | 112 | Invalid parameter | - | Malformatted post body |
