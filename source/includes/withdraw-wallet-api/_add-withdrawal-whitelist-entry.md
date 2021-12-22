## POST / Add Withdrawal Whitelist Entry

``` shell
# Post body
{
  "items": [
    {
      "address": "GCIFMEYIEWSX3K6EOPMEJ3FHW5AAPD6NW76J7LPBRAKD4JZKTISKUPHJ",
      "memo": "865314",
      "user_id": "USER001"
    }
  ]
}

curl -X POST -H "Content-Type: application/json" -d '{"items":[{"address":"GCIFMEYIEWSX3K6EOPMEJ3FHW5AAPD6NW76J7LPBRAKD4JZKTISKUPHJ","memo":"85666","user_id":"USER002"}]}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/whitelist

# An example of a successful response:
{
  "added_items": [
    {
      "address": "GCIFMEYIEWSX3K6EOPMEJ3FHW5AAPD6NW76J7LPBRAKD4JZKTISKUPHJ",
      "memo": "865314",
      "user_id": "USER001"
    }
  ]
}
```

Add an outgoing address to the withdrawal wallet's whitelist.


### Request
**POST** /v1/sofa/wallets/`WALLET_ID`/sender/whitelist

<aside class="notice">
 WALLET_ID must be a withdrawal wallet ID
</aside>


---


The request includes the following parameters:

### Post body

| Field | Type  | Note | Description |
| :---  | :---  | :--- | :---        |
| items | array | required | Specify the whitelist entries |
| address | string | required | The outgoing address |
| memo | string | optional | The memo of the outgoing address |
| user_id | string | optional, max length `255` | The custom user ID of the outgoing address |


### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| added_items | array | Array of added whitelist entries |

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
| 400 | 703 | Operation failed | invalid address: {INVALID_ADDRESS} | The address format does not comply with the cryptocurrency specification |
| 400 | 703 | Operation failed | invalid user id: {INVALID_USER_ID} | The length of the user ID exceeds 255 characters |
| 400 | 703 | Operation failed | this wallet does not support memo | The cryptocurrency does not support memo |
| 400 | 945 | The max length of BNB memo is 256 chars | - | Reached the limit of the length of BNB memo |
| 400 | 946 | The max length of EOS memo is 128 chars | - | Reached the limit of the length of EOS memo |
| 400 | 947 | The max length of XRP destination tag is 20 chars | - | Reached the limit of the length of XRP destination tag |
| 400 | 948 | The max length of XLM memo is 20 chars | - | Reached the limit of the length of XLM memo |
| 400 | 818 | Destination Tag must be integer | - | Wrong XRP destination tag format |
| 404 | 304 | Wallet ID invalid | - | The wallet is not allowed to perform this request |
