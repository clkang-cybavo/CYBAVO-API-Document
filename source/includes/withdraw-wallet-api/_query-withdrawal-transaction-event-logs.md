## GET / Query Withdrawal Transaction Event Logs

``` shell

curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/transactions/eventlog?txid={TXID}

# An example of a successful response:
{
  "logs": [
    {
      "address": "0xeCb258697e1A4B1fE11A43fC93bD4907f1EC8c04",
      "block_hash": "0x37b6b94b8fac1eb810ddda89ceedabd37f1017f671fe41a245dc6558a25cb4bf",
      "block_number": 11173598,
      "data": "000000000000000000000000000000000000000000000000000000003b9aca00",
      "index": 0,
      "removed": false,
      "topics": [
        "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
        "0x000000000000000000000000a952d7fc7965bec3cb03c79084236534ee2ab3cd",
        "0x000000000000000000000000d5909bacfc1fad78e4e45e9e2fef8b4e61c8fd0d"
      ],
      "tx_hash": "0xe82f0be9b30d840eca64e28e6f7562b7352b3b54519a7e63fb8f6d4609194bb9",
      "tx_index": 0
    }
  ]
}

```

Query event logs of a withdrawal transaction by transaction hash.


### Request
**GET** /v1/sofa/wallets/`WALLET_ID`/sender/transactions/eventlog?txid=`TXID`

<aside class="notice">
 WALLET_ID must be a withdrawal wallet ID
</aside>


---

### Query Parameters

| Field | Type | Note | Description |
| :---  | :--- | :--- | :---        |
| txid | string | required | Representing the transaction hash to query the event log |


### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| logs | array | The event logs |
| address | string | From which this event originated from |
| block_hash | string | Hash of the block where this transaction was in |
| block_number | uint64 | Block number where this transaction was in |
| data | string | The data containing non-indexed log parameter |
| index | uint | Integer of the event index position in the block |
| removed | boolean | Indicate the log is removed (due to chain reorg)  |
| topics | array | An array of values which must each appear in the log entries |
| tx_hash | string | Hash of the transaction |
| tx_index | uint | Integer of the transactions index position in the block |


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
| 400 | 112 | Invalid parameter | - | Missing necessary parameters |
| 400 | 112 | Invalid parameter | {TXID} not found | No relevant withdrawal request to TXID |
| 400 | 112 | Invalid parameter | {TXID} not in blockchain | Only in-chain withdrawal transactions allowed to query event log |
| 400 | 303 | Invalid currency | - | Not supported cryptocurrency to query event log |
