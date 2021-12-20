## POST / Cancel Withdrawal Request

``` shell
# The prefix is 888888_ in following sample request.
curl -X POST http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/transactions/{ORDER_ID}/cancel

# An example of the response
# The HTTP 200 means the withdrawal request has been cancelled successfully.


```

To cancel the withdrawal request which state is `Init` or `Failed`. The request state can be checked on web control panel or query through this [API](#query-withdrawal-callback-detail) (represents `state` = 0 or 5 ).

##### Request
**POST** /v1/sofa/wallets/`WALLET_ID`/sender/transactions/`ORDER_ID`/cancel

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>

---

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
| 403 | 177 | Illegal state | - | The {ORDER\_ID} withdrawal request is not in `Init` or `Failed` state |
| 403 | 385   | API Secret not valid | - | Invalid API code permission |
| 404 | 304 | Wallet ID invalid | - | The {ORDER\_ID} not found |
