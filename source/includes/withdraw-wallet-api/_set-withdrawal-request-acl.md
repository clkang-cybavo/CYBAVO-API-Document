## POST / Set Withdrawal Request ACL

``` shell
# Post body
{
  "acl": "192.168.101.55"
}
curl -X POST -H "Content-Type: application/json" -d '{"acl":"192.168.101.55"}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/transactions/acl

# An example of a successful response:
{
  "result": 1
}

```

Set an authorized IP to the withdrawal request ACL dynamically.

### Request
**POST** /v1/sofa/wallets/`WALLET_ID`/sender/transactions/acl

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>



---

The request includes the following parameters:

### Post body

| Field | Type | Note | Description |
| :---  | :--- | :--- | :---        |
| acl | string | requried | Specify an authorized IP in IPv4 format |


### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| result | int | Specify a successful API call |


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
| 404 | 304 | Wallet ID invalid | - | The wallet is invalid to perform this API call |
| 400 | 180 | Invalid format | - | The acl field is empty or does not conform to the IPv4 format |
| 400 | 180 | Operation failed | ACL has been set via web | The static ACL is not empty |
