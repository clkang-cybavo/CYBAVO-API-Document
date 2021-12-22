## POST / Check Withdrawal Whitelist

``` shell
# Post body
{
  "address": "GCIFMEYIEWSX3K6EOPMEJ3FHW5AAPD6NW76J7LPBRAKD4JZKTISKUPHJ",
  "memo": "865314",
  "user_id": "USER001"
}

curl -X POST -H "Content-Type: application/json" -d '{"address":"GCIFMEYIEWSX3K6EOPMEJ3FHW5AAPD6NW76J7LPBRAKD4JZKTISKUPHJ","memo":"85666","user_id":"USER002"}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/whitelist/check

# An example of a successful response:
{
  "address": "0x79D6660b2aB1d37AD5D11C2ca2B3EBba7Efd13F6",
  "create_time": "2020-12-30T13:09:39Z",
  "effective": true,
  "effective_time": "2020-12-30T13:09:39Z",
  "memo": "",
  "state": 1,
  "update_time": "2020-12-30T13:09:39Z",
  "user_id": "USER001"
}
```

Check the withdrawal whitelist entry status in the withdrawal whitelist.

### Request
**POST** /v1/sofa/wallets/`WALLET_ID`/sender/whitelist/check

<aside class="notice">
 WALLET_ID must be a withdrawal wallet ID
</aside>


---

The request includes the following parameters:

### Post body

| Field | Type  | Note | Description |
| :---  | :---  | :--- | :---        |
| address | string | required | The inquiry whitelist entry address |
| memo | string | optional | The memo of the whitelist entry |
| user_id | string | optional | The custom user ID of the whitelist entry |

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| address | string | The inquiry whitelist entry address |
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
| 400 | 112 | Invalid parameter | - | Malformatted post body |
| 400 | 703 | Operation failed | not found | Cannot find the inquiry whitelist entry |
| 400 | 703 | Operation failed | invalid address: {INVALID_ADDRESS} | The address format does not comply with the cryptocurrency specification |
| 400 | 703 | Operation failed | invalid user id: {INVALID_USER_ID} | The length of the user ID exceeds 255 characters |
| 400 | 703 | Operation failed | this wallet does not support memo | The cryptocurrency does not support memo |
| 404 | 304 | Wallet ID invalid | - | The wallet is not allowed to perform this request |
