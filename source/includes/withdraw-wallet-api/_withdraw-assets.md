# Withdraw Wallet API

## POST / Withdraw Assets

``` shell
# The prefix is 888888_ in following sample request.
# Sample Post body
{
  "requests": [
    {
      "order_id": "888888_1",
      "address": "0x83eE561B2aBD000FF00d6ca22f38b29d4a760d4D",
      "amount": "0.0001",
      "memo": "memo-001",
      "user_id": "USER01",
      "message": "message-001",
      "block_average_fee": 5
    },
    {
      "order_id": "888888_2",
      "address": "0xf16B7B8900F0d2f682e0FFe207a553F52B6C7015",
      "amount": "0.0002",
      "manual_fee": 50
    },
    {
      "order_id": "888888_3",
      "address": "0x9638fa816ccd35389a9a98a997ee08b5321f3eb9",
      "amount": "0.0002",
      "message": "message-003"
    },
    {
      "order_id": "888888_4",
      "address": "0x2386b18e76184367b844a402332703dd2eec2a90",
      "amount": "0",
      "contract_abi":"create:0x000000000000000000000000000000000000000000000000000000000000138800000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000000"
      "user_id": "USER04"
    },
    {
      "order_id": "888888_5",
      "address": "0x2386b18e76184367b844a402332703dd2eec2a90",
      "amount": "1",
      "token_id": "985552421"
    }
  ],
  "ignore_black_list": false
}

# example of curl request:
curl -X POST -H "Content-Type: application/json" -d '{"requests":[{"order_id":"888888_1",\
"address":"0x60589A749AAC632e9A830c8aBE042D1899d8Dd15","amount":"0.0001","memo":"memo-001",\
"user_id":"USER01","message":"message-001"},{"order_id":"888888_2",\
"address":"0xf16B7B8900F0d2f682e0FFe207a553F52B6C7015","amount":"0.0002","memo":"memo-002",\
"user_id":"USER01","message":"message-002"}]}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/transactions

# An example of a successful response:
{
  "results": {
    "1": 20000000001,
    "2": 20000000002
  }
}


# An example response of the request contains problematic addresses:
{
    "error_code": 827,
    "error": "Outgoing address in black list, abort transaction",
    "blacklist": {
        "0x83eE561B2aBD000FF00d6ca22f38b29d4a760d4D": [
            "Involve phishing activity",
            "Involve cybercrime related"
        ]
    }
}
```

To withdraw assets from an withdrawal wallet, the caller must to provide an unique **order_id** for each request, the CYBAVO SOFA system will send the callback with the unique **order_id** when the withdrawal is success (from `in pool` state to `in chain` state). 

By default, the withdraw API will perform the address check to verify that the outgoing address is good or not. If the address in the request is marked as a problematic address, the request will be aborted. The error message will identify the problematic addresses. Set the `ignore_black_list` to true to skip the address check.

The withdrawal API can also interact with the contracts (ERC/BEP 721/1155) deployed in the SOFA system.


### Request
**POST** /v1/sofa/wallets/`WALLET_ID`/sender/transactions

<aside class="notice">
 WALLET_ID must be a withdrawal wallet ID
</aside>

<aside class="notice">
The order_id must be prefixed. Find prefix from corresponding wallet detail on web control panel.
</aside>

<aside class="notice">
If withdraw BNB or XRP, this API will check whether the destination addresse needs memo / destination tag or not. If the address does need memo / destination tag, the API will fail without memo / destination tag specified.
</aside>


---

The request includes the following parameters:

### Post body

| Field | Type  | Note | Description |
| :---  | :---  | :--- | :---        |
| order_id | string | required, max `255` chars | Specify an unique ID, order ID must be prefixed |
| address | string | required | Outgoing address (`address` must be a contract address, if the contract_abi is not empty) |
| amount | string | required | Withdrawal amount |
| contract_abi | string | required, if calls contract ABI | Specify the ABI method and the parameters, in the format `ABI_method:parameters` |
| memo | string | optional | Memo on blockchain (This memo will be sent to blockchain). Refer to [Memo Requirement](#memo-requirement) |
| user_id | string | optional | Specify certain user |
| message | string | optional | Message (This message only saved on CYBAVO, not sent to blockchain) |
| block\_average_fee | int | optional, range `1~100` | Use average blockchain fee within latest N blocks. This option does not work for XRP, XLM, BNB, DOGE, EOS, TRX, ADA, DOT and SOL cryptocurrencies. |
| manual_fee | int | optional, range `1~2000` | Specify blockchain fee in smallest unit of wallet currency **`(For ETH/BSC/HECO/OKT/OP/ARB/CELO/FTM/PALM, the unit is gwei)`**. This option does not work for XRP, XLM, BNB, DOGE, EOS, TRX, ADA, DOT and SOL cryptocurrencies. |
| token_id | string | optional | Specify the token ID to be transferred |
| ignore\_black_list| boolean | optional, default `false` | After setting, the address check will not be performed. |
| ignore\_gas\_estimate_fail | boolean | optional, default `false` | **FOR DEBUG PURPOSE ONLY**. After setting, the ABI EVM gas estimation will not be performed. |

- The order_id must be prefixed. Find prefix from corresponding wallet detail on web control panel

- block\_average\_fee and manual_fee are mutually exclusive configurations. If neither of these fields is set, the fee will refer to corresponding withdrawal policy of the withdrawal wallet.

- The format of the `contract_abi` is `ABI_method:hex_parameters`, for example: create:0x000000000000000000000000000000000000000000000000000000000000138800000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000000. The parameters must be encoded by [web3.eth.abi.encodeParameters() of web3.js](https://web3js.readthedocs.io/en/v1.3.4/web3-eth-abi.html#encodeparameters).

- Only ERC721/1155 wallet can use `token_id` to transfer token. For ERC721 wallets, if `token_id` is specified, the amount will be ignored.

- The `block\_average_fee` and `manual_fee ` do not work for XRP, XLM, BNB, DOGE, EOS, TRX, ADA, DOT and SOL cryptocurrencies.

### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| results | array | Array of withdraw result (order ID/withdraw transaction ID pair), if succeeds |


An example response of the request contains problematic addresses:

| Field | Type  | Description |
| :---  | :---  | :---        |
| error_code | int | The error code |
| error | string | The error message |
| blacklist | object | The object describes all problematic addresses and their causes. |


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
| 403 | 827 | Outgoing address in black list, abort transaction | - | Some outgoing addresses are blacklisted, examine the response 'blacklist' field for detailed information |
| 400 | 112 | Invalid parameter | - | Malformatted post body |
| 400 | 955 | There is no content in your withdrawal request, please check your input | - | The post body of request doesn't conform the API request specification |
| 400 | 703 | Operation failed | order_id must start with {ORDERID\_PREFIX} | The prefix of order_id is incorrect |
| 400 | 703 | Operation failed | order_id: {ORDER\_ID} - the character \\ or / is prohibited | {ORDER\_ID} is invalid |
| 400 | 703 | Operation failed | order_id: {ORDER\_ID} is invalid | {ORDER\_ID} is invalid |
| 400 | 703 | Operation failed | order_id: {ORDER\_ID} - memo is required | The outgoing address of {ORDER\_ID} needs memo specified |
| 400 | 703 | Operation failed | order_id: {ORDER\_ID} - destination tag is required | The outgoing address of {ORDER\_ID} needs destination tag specified |
| 400 | 703 | Operation failed | order_id: {ORDER\_ID} - invalid block\_average\_fee | The block\_average\_fee is out of range |
| 400 | 703 | Operation failed | order_id: {ORDER\_ID} - invalid manual\_fee | The manual\_fee is out of range |
| 400 | 399 | Duplicated entry: {ORDER\_ID} | - | The {ORDER\_ID} is duplicated |
| 400 | 945 | The max length of BNB memo is 256 chars | - | Reached the limit of the length of BNB memo |
| 400 | 946 | The max length of EOS memo is 128 chars | - | Reached the limit of the length of EOS memo |
| 400 | 947 | The max length of XRP destination tag is 20 chars | - | Reached the limit of the length of XRP destination tag |
| 400 | 948 | The max length of XLM memo is 20 chars | - | Reached the limit of the length of XLM memo |
| 400 | 818 | Destination Tag must be integer | - | Wrong XRP destination tag format |
| 400 | 944 | The max length of order id is 255 chars | - | Reached the limit of the length of order_id |
| 400 | 703 | Operation failed | Detailed error message | Failed to connect to authentication callback URL |
| 400 | 703 | Operation failed | The withdrawal request has been rejected, {RESPONSE_BODY} | The withdrawal request has been rejected by the authentication callback |
| 400 | 703 | Operation failed | The withdrawal request has been rejected, unexpected response {HTTP\_CODE}: {RESPONSE_BODY} | The authentication callback URL returned status code other than 200 or 400 |
| 400 | 703 | Operation failed | Unrecognized response: {RESPONSE_BODY}, 'OK' expected | The returned status code is 200 but the body is not **OK** |
| 400 | 703 | Operation failed | request IP ({IPv4}) not in ACL | The request IP not in the withdrawal ACL |
| 400 | 703 | Operation failed | invalid amount {AMOUNT} | The requested amount is not a valid number |
| 400 | 703 | Operation failed | invalid amount decimals {AMOUNT} | The decimals of the requested amount exceeds the valid range |
| 404 | 304 | Wallet ID invalid | - | The wallet is not allowed to perform this request |
| 404 | 312 | Policy not found | no active withdrawal policy found | No active withdrawal policy found |
