## POST / Sign Message

``` shell
# Post body
{
  "message": "This is proof that I, user A, have access to this address."
}

curl -X POST -H "Content-Type: application/json" -d '{"message":"This is proof that I, user A, have access to this address."}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/signmessage

# An example of a successful response:
{
  "signed_message": "0xf296a678ce1d577acbee25119b7be821db70e960d6e65ef73fb1e50fa832759d27d35df2dd309be07cb5a9b9f6c87f5eeae11ff56b995e0b32d6288b1039555b2a"
}

```

Sign message, equivalent to `eth_sign`.

### Request
**POST** /v1/sofa/wallets/`WALLET_ID`/signmessage

<aside class="notice">
 WALLET_ID must be a withdrawal wallet ID
</aside>



---

The request includes the following parameters:

### Post body

| Field | Type  | Note | Description |
| :---  | :---  | :--- | :---        |
| message | string | required | Message to be signed |


### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| signed_message | string | Signed message |


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
| 404 | 303 | Invalid currency | - | The wallet is not allowed to perform this request |
| 404 | 304 | Wallet ID invalid | mapped wallet not supported | - |