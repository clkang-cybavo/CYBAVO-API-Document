## GET &nbsp;Query Pool Address

``` shell

curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/pooladdress

# An example of a successful response:
{
  "address": "0x36099775afa8d6363aC8e5d0fC698306C021a858"
}

```

Get the pool address of a deposit wallet. The pool address has different functionality in different cryptocurrencies.

- In BTC or ETH, the cryptocurrency in the pool address will be used to pay for token transfer(ex. USDT-Omni, ERC20).

- BCH/BSV need at least 0.01 BCH/BSV in the pool address for polluting the non-fork coin and make the collection successfully.

 - In EOS, XRP, XLM or BNB, the pool address is the user's deposit address. All user deposits will be distinguished by memo / tag field.

- LTC, DOGE, DASH, DOT(WND), FIL. SOL and ADA does not support pool address.



##### Request
**GET** /v1/sofa/wallets/`WALLET_ID`/pooladdress

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| address  | string | Pool address of wallet |

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
| 400 | 703 | this wallet does not support pool address | - | - |
| 404 | 304 | Wallet ID invalid | - | The wallet is not allowed to perform this request |
