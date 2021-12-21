## GET / Call Contract Read ABI

``` shell
# Post body
{
  "message": "This is proof that I, user A, have access to this address."
}

curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/contract/read?contract=0xad6d458402f60fd3bd25163575031acdce07538d&data=0xdd62ed3e000000000000000000000000d11bd6e308b8dc1c5243d54cf41a427ca0f46943000000000000000000000000e592427a0aece92de3edee1f18e0157c05861564

# An example of a successful response:
{
  "output": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
}

```

Executes a contract read ABI call.

### Request
**GET** /v1/sofa/wallets/`WALLET_ID`/contract/read?contract=`contract_address`&data=`data`

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>



---

### Query Parameters

The request includes the following parameters:

| Field | Type | Note | Description |
| :---  | :--- | :--- | :---        |
| contract | string | required | Representing the contract to interact with |
| data | string | requried | The hash of the method signature and encoded parameters |

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| output | string | The output of read ABI call |

### The `output` can be decoded by [web3.eth.abi.decodeparameters() of web3.js](https://web3js.readthedocs.io/en/v1.3.4/web3-eth-abi.html#decodeparameters).

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
| 404 | 304 | Wallet ID invalid | wrong wallet type | - |
| 404 | 304 | Wallet ID invalid | archived wallet | - |
| 404 | 304 | Wallet ID invalid | mapped wallet not supported | - |
| 400 | 112 | Invalid parameter | - | Missing necessary parameters |
| 400 | 112 | Operation failed | invalid address: {contract_address} | The contract address does not conform to the cryptocurrency format |
| 400 | 112 | Invalid parameter | no contract policy | There is no contract policy of the given wallet |
| 400 | 112 | Invalid parameter | unsupported contract | There is no contract policy of the given contract address |
| 400 | 303 | Invalid currency | - | Not supported cryptocurrency to call contract read ABI |
