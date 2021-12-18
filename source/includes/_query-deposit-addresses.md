## GET / Query Deposit Addresses


``` shell

curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/addresses?start_index=0&request_number=1000

#--- THEN ---

curl http://localhost:8889/v1/sofa/wallets/179654/addresses?start_index=3&request_number=3


# An example of a successful response:
{
  "wallet_id": 179654,
  "wallet_count": 6,
  "wallet_address": [
    {
      "currency": 60,
      "token_address": "",
      "address": "0x8c42fD03A5cfba7C3Cd97AB8a09e1a3137Ef33C3",
      "memo": ""
    },
    {
      "currency": 60,
      "token_address": "",
      "address": "0x4d3EB54b602BF4985CE457089F9fB084Af597A2C",
      "memo": ""
    },
    {
      "currency": 60,
      "token_address": "",
      "address": "0x74dc3fB523295C87C0b93E48744Ce94fe3a8Ef5e",
      "memo": ""
    }
  ]
}

#--- THEN ---

{
  "wallet_id": 179654,
  "wallet_count": 6,
  "wallet_address": [
    {
      "currency": 60,
      "token_address": "",
      "address": "0x6d68443D6564cF257A48c1b16aa6d0EF13c5A719",
      "memo": ""
    },
    {
      "currency": 60,
      "token_address": "",
      "address": "0x26F103322B6f0ed2D35B85F1611589c92F023986",
      "memo": ""
    },
    {
      "currency": 60,
      "token_address": "",
      "address": "0x2b91918Bee4411DaD6293EA5d6D38251E72723Ca",
      "memo": ""
    }
  ]
}

```
Query the deposit addresses created by the [Create Deposit Addresses](#post-nbsp-create-deposit-addresses) API.

##### Request
**GET** /v1/sofa/wallets/`WALLET_ID`/addresses?start\_index=`from`&request\_number=`count`

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>

The request includes the following parameters:

| Field | Type  | Note | Description |
| :---  | :---  | :--- | :---        |
| start_index | int | optional, default `0` | Specify address start index |
| request_number | int | optional, default `1000`, max `5000` | Request address count |


The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| wallet_id | int64 | ID of request wallet |
| wallet_address | array | Array of wallet addresses |
| wallet_count | int64 | Total count of deposit addresses |


### Refer to [Currency Definition](#currency-definition) or [here](https://github.com/satoshilabs/slips/blob/master/slip-0044.md) for more detailed currency definitions

- If this is an ETH contract collection deposit wallet, only the deployed address will be returned.

#### Error Code

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
| 403 | 112 | Invalid parameter | - | The count and the count of memos mismatched |
| 403 | 385   | API Secret not valid | - | Invalid API code permission |
| 403 | 706 | Exceed max allow wallet limitation, Upgrade your SKU to get more wallets | - | Reached the limit of the total number of deposit addresses |
| 400 | 421 | Mapped(Token) wallet not allow to create deposit addresses, please create the deposit wallet in parent wallet, the address will be synced to mapped wallet automatically | - | Only the parent wallet can create deposit addresses |
| 400 | 500 | insufficient fund | - | Insufficient balance to deploy collection contract |
| 400 | 703 | Operation failed | Error message returned by JSON parser | Malformatted post body |
| 400 | 818 | Destination Tag must be integer | - | Wrong XRP destination tag format |
| 400 | 945 | The max length of BNB memo is 256 chars | - | Reached the limit of the length of BNB memo |
| 400 | 946 | The max length of EOS memo is 128 chars | - | Reached the limit of the length of EOS memo |
| 400 | 947 | The max length of XRP destination tag is 20 chars | - | Reached the limit of the length of XRP destination tag |
| 400 | 948 | The max length of XLM memo is 20 chars | - | Reached the limit of the length of XLM memo |
| 404 | 304 | Wallet ID invalid | archived wallet or wrong wallet type | The wallet is not allowed to perform this request |

