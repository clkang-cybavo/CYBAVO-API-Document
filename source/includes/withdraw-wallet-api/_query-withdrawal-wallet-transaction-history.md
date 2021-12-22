## GET / Query Withdrawal Wallet Transaction History

``` shell

curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/transactions

# An example of a successful response:
{
  "total_count": 169,
  "transactions": [
    {
      "amount": "0.1",
      "block_height": 10813730,
      "block_time": "2021-08-11T06:13:01Z",
      "blocklist_tags": [],
      "fee": "0.000693",
      "from_address": "0xaa0cA2f9bA3A33a915a27e289C9719adB2ad7d73",
      "memo": "",
      "out": true,
      "source": "",
      "state": 1,
      "to_address": "0x79D6660b2aB1d37AD5D11C2ca2B3EBba7Efd13F6",
      "txid": "0xe3607325e3b7c0190089d1fb41ce9fa059858c6b2e5dd220e55ba46707fc38f0"
    },
    {
      "amount": "1",
      "block_height": 10811102,
      "block_time": "2021-08-10T17:24:21Z",
      "blocklist_tags": [],
      "fee": "0.000021",
      "from_address": "0xaa0cA2f9bA3A33a915a27e289C9719adB2ad7d73",
      "memo": "",
      "out": true,
      "source": "withdraw-api",
      "state": 1,
      "to_address": "0x8382Cc1B05649AfBe179e341179fa869C2A9862b",
      "txid": "0x19657382aa16520c32eef0dacc0f16d78e9105e83d37d126b4f6687c0d651859"
    },
  ]
}
```

Get transaction history of withdrawal wallets.

### Request
**GET** /v1/sofa/wallets/`WALLET_ID`/sender/transactions?from\_time=`from`&to\_time=`to`&start\_index=`start`&request_number=`count`

<aside class="notice">
 WALLET_ID must be a withdrawal wallet ID
</aside>

---

### Query Parameters

| Field | Type | Note | Description |
| :---  | :--- | :--- | :---        |
| from_time | int64 | optional, default `0` | Start date (unix time in UTC) |
| to_time | int64 | optional, default `current time` | End date (unix time in UTC) |
| start_index | int | optional, default `0` | Index of starting transaction record |
| request_number | int | optional, default `10`, max `500` | Count of returning transaction record |

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| total_count | int | Total transactions in specified date duration |
| transactions | array | Array of transaction record |
| txid | string | Transaction ID |
| from_address | string | Sender address of the transaction |
| to_address | string | Recipient address of the transaction |
| out | boolean | True means outgoing transaction |
| amount | string | Transaction amount |
| blocklist_tags | array | The tags of CYBAVO AML detection. If `out` is true, the `to_address` is tagged. Otherwise, the `from_address` is tagged |
| block_height | int64 | The block height |
| block_time | time | When was the transaction packed into block (in chain) in UTC time |
| fee | string | Transaction blockchain fee |
| memo | string | Memo of the transaction |
| source | string | `withdraw-api` means that the transaction was triggered by the withdrawal API, otherwise it was triggered from the web withdrawal UI |
| state | int | Refer to [State Definition](#state-definition) bellow |

<a name="state-definition"></a>
### State Definition

| ID   | Description |
| :--- | :---        |
| 1 | Success, the transaction status is successful |
| 2 | Failed, the transaction status is failed |
| 3 | Invalid, the transaction status is successful but is identified as invalid by the SOFA system |

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
| 400 | 112 | Invalid parameter | - | `from_time` or `to_time` is invalid |