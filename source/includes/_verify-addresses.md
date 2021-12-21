## POST / Verify Addresses

``` shell

# Post body
{
  "addresses": [
    "0x635B4764D1939DfAcD3a8014726159abC277BecC",
    "1CK6KHY6MHgYvmRQ4PAafKYDrg1ejbH1cE"
  ]
}

curl -X POST -H "Content-Type: application/json" -d '{"addresses":["0x635B4764D1939DfAcD3a8014726159abC277BecC","1CK6KHY6MHgYvmRQ4PAafKYDrg1ejbH1cE"]}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/addresses/verify

# An example of a successful response:
{
  "result": [
    {
      "address": "0x635B4764D1939DfAcD3a8014726159abC277BecC",
      "valid": true,
      "must_need_memo": false
    },
    {
      "address": "1CK6KHY6MHgYvmRQ4PAafKYDrg1ejbH1cE",
      "valid": false,
      "must_need_memo": false
    }
  ]
}
```

Check if the address conforms to the wallet cryptocurrency address format (for example, ETH must have the prefix 0x, BTC should start with 1, 3 or bc1, etc).

### If the wallet's cryptocurrency is BNB or XRP, there will be a `must_need_memo` flag to indicate whether the address needs a memo / destinati

### Request

**POST** /v1/sofa/wallets/`WALLET_ID`/addresses/verify

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>

---

### Post body

| Field | Type  | Note | Description |
| :---  | :---  | :--- | :---        |
| addresses | array | requried | Specify the address for verification |

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| result | array | Array of addresses' verification result |
| must\_need\_memo | boolean | Indicate whether the address needs a memo / destination tag when transferring cryptocurrency to that address |

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
