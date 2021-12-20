## POST / Query Deposit Address Label

``` shell

curl -X POST -H "Content-Type: application/json" -d '{"addresses":["0x2B974a3De0b491bB26e0bF146E6cDaC36EFD574a","0xF401AC94D9672e79c68e56A6f822b666E5A7d644"]}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/addresses/get_labels

# An example of a successful response:
{
  "labels": {
    "0x2B974a3De0b491bB26e0bF146E6cDaC36EFD574a": "take-some-notes",
    "0xF401AC94D9672e79c68e56A6f822b666E5A7d644": ""
  }
}

```

Query the labels of the deposit addresses.


##### Request
**POST** /v1/sofa/wallets/`WALLET_ID`/addresses/get_labels

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>


---


The request includes the following parameters:

###### Post body

| Field | Type  | Note | Description |
| :---  | :---  | :---  | :---        |
| addresses | array | required | Specify the addresses to query labels |


The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| labels | key-value pairs | The address-label pairs |

<aside class="notice">
 If the address can not be found, it will not be listed in the response.
</aside>

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
