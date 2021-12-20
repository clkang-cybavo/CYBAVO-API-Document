## GET / Query Pool Address Balance

``` shell

curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/pooladdress/balance


# An example of a successful response:
{
  "balance": "0.515",
  "currency": 60,
  "unconfirm_balance": "0",
  "wallet_address": "0xb6ad80c96D093EA584AfcB9443927812d3e4Bd94"
}

```

### Request
**GET** /v1/sofa/wallets/`WALLET_ID`/addresses?start\_index=`from`&request\_number=`count`

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>


---

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| balance | string | Pool address balance |
| unconfirm\_balance | string | Unconfirmed pool address balance |
| currency | int64 | Cryptocurrency of the wallet |
| wallet_address  | string | Pool address of the wallet |


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
| 404 | 304 | Wallet ID invalid | - | The wallet is not allowed to perform this request |
