## GET / Query Vault Wallet Transaction History

``` shell

curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/transactions?start_index=0&from_time=1559664000&to_time=1562255999&request_number=8

# An example of a successful response:
{
  "transaction_count": 3,
  "transaction_item": [
    {
      "issue_user_id": 3,
      "issue_user_name": "wallet owner (user@gmail.com)",
      "description": "TO SND",
      "wallet_id": 488471,
      "wallet_name": "BNB I",
      "wallet_address": "tbnb1655kasahedvaeudaeq6jggr7kal8qgwygu9xqk",
      "token_address": "",
      "txid": "3E6D61D1D39FA5DD3B86C2C28FFAD3D98CD7AFDB62346468D3C4FFE710CAAF85",
      "currency": 714,
      "currency_name": "BNB",
      "outgoing_address": "tbnb1f805kv6z8nq2whrcnkagjte3jjss2sxf2rfls0",
      "outgoing_address_name": "BNB SND",
      "amount": "2",
      "fee": "0",
      "txno": 100087,
      "approval_item": [
        {
          "approval_id": 3,
          "approval_user": "wallet owner (user@gmail.com)",
          "approval_time": 1562210142,
          "user_message": "",
          "level": 0,
          "owner": 1,
          "confirm": 1,
          "state": 2,
          "error_code": 0
        }
      ],
      "state": 2,
      "create_time": 1562210129,
      "transaction_time": 1562210142,
      "scheduled_name": "",
      "transaction_type": 0,
      "eos_transaction_type": 0,
      "real_amount": "2",
      "chain_fee": "0.000375",
      "platform_fee": "0",
      "tx_category": "",
      "memo": "TO SND"
    }
  ]
}

```

Get transaction history of vault wallets.

### Request

**GET** /v1/sofa/wallets/`WALLET_ID`/transactions?from\_time=`from`&to\_time=`to`&start\_index=`start`&request_number=`count`&state=`state`

<aside class="notice">
 WALLET_ID must be a deposit wallet ID
</aside>

---

### Query Parameters

| Field | Type | Note | Description |
| :---  | :--- | :--- | :---        |
| from_time | int64 | optional, default `0` | Start date (unix time in UTC) |
| to_time | int64 | optional, default `current time` | End date (unix time in UTC) |
| start_index | int | optional, default `0` | Index of starting transaction record |
| request_number | int | optional, default `1000`, max `5000` | Count of returning transaction record |
| state | int | optional, default `-1` | Refer to [Transaction State Filter Definition](#transaction-state-filter) bellow |

<a name="transaction-state-filter"></a>
### Transaction State Filter Definition

| ID   | Description |
| :--- | :---        |
| 0 | WaitApproval |
| 1 | Rejected |
| 2 | Approved |
| 3 | Failed |
| 4 | NextLevel |
| 5 | Cancelled |
| 6 | BatchDone |
| -1 | All states (for inquiry) |


### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| notifications | array | Arrary of callbacks, refer to [Callback Definition](#callback-definition) |

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
