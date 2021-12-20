## GET / Query Deployed Contract Deposit Addresses

``` shell
curl 'http://localhost:8889/v1/sofa/wallets/179654/addresses/contract_txid?txids=0xe6dfe0d283690f636df5ea4b9df25552e6b576b88887bfb5837016cdd696e754,0xdb18fd33c9a6809bfc341a1c0b2c092be5a360f394c85367f9cf316579281ab4'


# An example of a successful response:
{
  "addresses": {
    "0xdb18fd33c9a6809bfc341a1c0b2c092be5a360f394c85367f9cf316579281ab4": {
      "address": "0x00926cE2BbF56317c72234a0Fb8A65A1A15F7103",
      "currency": 60,
      "memo": "",
      "token_address": ""
    },
    "0xe6dfe0d283690f636df5ea4b9df25552e6b576b88887bfb5837016cdd696e754": {
      "address": "0xf3747e3edbd8B8414718dd51330415c171e79208",
      "currency": 60,
      "memo": "",
      "token_address": ""
    }
  }
}

```

Query deployed contract deposit addresses created by the [Create Deposit Addresses](#post-nbsp-create-deposit-addresses) API.


##### Request
**GET** /v1/sofa/wallets/`WALLET_ID`/addresses?start\_index=`from`&request\_number=`count`

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>
<aside class="notice">
 Only deployed addresses will be returned
</aside>

---


### Request Format

The request includes the following parameters:

| Field | Type  | Note | Description |
| :---  | :---  | :--- | :---        |
| txids | string | requried, max `10` transaction IDs | Transaction ID used to deploy collection contract |

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| addresses | map object | The map KEY is Transaction ID used to deploy collection contract and the map VALUE is the address information |


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
