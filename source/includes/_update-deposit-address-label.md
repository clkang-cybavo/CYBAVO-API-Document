## POST / Update Deposit Address Label

``` shell

curl -X POST -H "Content-Type: application/json" -d '\
{"address":"0x2B974a3e0b491bB26e0bF146E6cDaC36EFD574a","label":"take-some-notes"}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/addresses/label


```

Update the label of the deposit address.

##### Request
**POST** /v1/sofa/wallets/`WALLET_ID`/addresses/label

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>
<aside class="notice">
The label will be automatically synced between the parent and child wallet.
</aside>


---

The request includes the following parameters:

###### Post body

| Field | Type  | Note | Description |
| :---  | :---  | :---  | :---        |
| address | string | required | Specify address to update the label |
| label | string | optional, set empty to clear the label | Specify the label of the address |


##### Response Format

Status code 200 represnts a successful operation


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
| 404 | 112 | Invalid parameter | - | The address can not be found |
