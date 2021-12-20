## DELETE / Remove Withdrawal Whitelist Entry

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

curl -X DELETE -H "Content-Type: application/json" -d '{"items":[{"address":"GCIFMEYIEWSX3K6EOPMEJ3FHW5AAPD6NW76J7LPBRAKD4JZKTISKUPHJ","memo":"85666","user_id":"USER002"}]}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/whitelist

# An example of a successful response:
{
  "removed_items": [
    {
      "address": "GCIFMEYIEWSX3K6EOPMEJ3FHW5AAPD6NW76J7LPBRAKD4JZKTISKUPHJ",
      "memo": "865314",
      "user_id": "USER001"
    }
  ]
}
```

Remove an outgoing address from the withdrawal wallet's whitelist.

##### Request
**DELETE** /v1/sofa/wallets/`WALLET_ID`/sender/whitelist

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>

<aside class="notice">
 Only the entries exactly matches all the fields will be removed.
</aside>


---

The request includes the following parameters:

###### Post body

| Field | Type  | Note | Description |
| :---  | :---  | :--- | :---        |
| items | array | required | Specify the whitelist entries |
| address | string | required | The outgoing address |
| memo | string | optional | The memo of the outgoing address |
| user_id | string | optional | The custom user ID of the outgoing address |


The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| removed_items | array | Array of removed whitelist entries |


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
