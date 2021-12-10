---
title: CYBAVO API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - go
  - java
  - javascript


includes:
  

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Kittn API
---
<a name="get-started"></a>
# Get Started

### How to deposit?
- Setup a deposit wallet and configure it (via web control panel)
	- Refer to CYBAVO VAULT SOFA User Manual for detailed steps.
- Request an API code/secret (via web control panel)
- Create deposit addresses (via CYBAVO SOFA API)
	- Refer to [Create deposit addresses](#create-deposit-addresses) API
- Waiting for the CYBAVO SOFA system detecting transactions to those deposit addresses
- Handle the deposit callback
	- Use the callback data to update certain data on your system.
	- <b>Security Enhancement</b>: Use the Query Callback Detail API to confirm the callback is sent from the CYBAVO SOFA system.
	- Refer to [Callback Integration](#callback-integration) for detailed information.

### How to withdraw?
- Setup a withdrawal wallet and configure it (via web control panel)
	- Refer to CYBAVO VAULT SOFA User Manual for detailed steps.
- Request an API code/secret (via web control panel)
- Make withdraw request (via CYBAVI SOFA API)
	- Refer to [Withdraw Assets](#withdraw-assets) API
	- <b>Security Enhancement</b>: Also set the withdrawal authentication callback URL to authorize the withdrawal requests sent to the CYBAVO SOFA system.
- Waiting for the CYBAVO SOFA system broadcasting transactions to blockchain
- Handle the withdrawal callback
	- Use the callback data to update certain data on your system.
	- Refer to [Callback Integration](#callback-integration) for detailed information.

### Try it now
- Use [mock server](#mock-server) to test CYBAVO SOFA API right away.

### Start integration
- To make a correct API call, refer to [API Authentication](#api-authentication).
- To handle callback correctly, refer to [Callback Integration](#callback-integration).


<a name="api-authentication"></a>
# API Authentication

- The CYBAVO SOFA system verifies all incoming requests. All requests must include X-API-CODE, X-CHECKSUM headers otherwise caller will get a 403 Forbidden error.

### How to acquire and refresh API code and secret
- Request the API code/secret from the **Wallet Details** page on the web control panel for the first time.
- A paired refresh code can be used in the [refresh API](#refresh-api-code) to acquire the new inactive API code/secret of the wallet.
	- Before the inactive API code is activated, the currently activated API code is still valid.
	- Once the paired API code becomes invalid, the paired refresh code will also become invalid.

### How to make a correct request?
- Put the API code in the X-API-CODE header.
	- Use the inactivated API code in any request will activate it automatically. Once activated, the currently activated API code will immediately become invalid.
	- Or you can explicitly call the [activation API](#activate-api-code) to activate the API code before use
- Calculate the checksum with the corresponding API secret and put the checksum in the X-CHECKSUM header.
  - The checksum calculation will use all the query parameters, the current timestamp, user-defined random string and the post body (if any).
- Please refer to the code snippet on the github project to know how to calculate the checksum.
	- [Go](https://github.com/CYBAVO/API_CHECKSUM_CALC/blob/main/go/checksum.go#L40)
	- [Java](https://github.com/CYBAVO/API_CHECKSUM_CALC/blob/main/java/checksum.java#L49)
	- [Javascript](https://github.com/CYBAVO/API_CHECKSUM_CALC/blob/main/javascript/checksum.js#L27)
	- [PHP](https://github.com/CYBAVO/API_CHECKSUM_CALC/blob/main/php/checksum.php#L27)
	- [C#](https://github.com/CYBAVO/API_CHECKSUM_CALC/blob/main/c%23/checksum.cs#L55)
	- [Python](https://github.com/CYBAVO/API_CHECKSUM_CALC/blob/main/python/checksum.py#L29)


<a name="readonly-api-code"></a>
# Read-only API Code

- A read-only API code can be used to call all the read functions of wallets.
	- All the read functions will be labeled `VIEW` in front of the API definition.
- Use [activation API](#activate-api-code) with the `WALLET_ID` set as `readonly` to activate a read-only API code.
	- The full API path is `/v1/sofa/wallets/readonly/apisecret/activate`
	- After activation, the API code will remain valid until it is replaced by a newly activated read-only API code.
- Use [listing API](#list-wallets) to list all wallets that can be accessed through a read-only API code.

# Callback
## Callback Integration

- Please note that the wallet must have an activated API code, otherwise no callback will be sent.
	- Use the [activation API](#activate-api-code) to activate an API code.
- How to distinguish between deposit and withdrawal callbacks?
	- Deposit Callback (callback type 1)
	  - The combination of **txid** and **vout_index** of the callback is unique, use this combined ID to identify the deposit request, not to use only the transaction ID (txid field). Because multiple deposit callbacks may have the same transaction ID, for example, BTC many-to-many transactions.
	- Withdrawal Callback (callback type 2)
	  - The **order_id** of the callback is unique, use this ID to identify the withdrawal request.

<aside class="notice">
It is important to distinguish between uniquecallbacks to avoid</br>
improper handling of deposit/ withdrawal requests.
</aside>

- To ensure that the callbacks have processed by callback handler, the CYBAVO SOFA system will continue to send the callbacks to the callback URL until a callback confirmation (HTTP/1.1 200 OK) is received or exceeds the number of retries (retry time interval: 1-3-5-15-45 mins).
	- If all attempts fail, the callback will be set to a failed state, the callback handler can call the [resend deposit callback](#resend-deposit-callbacks) or [resend withdrawal callback](#resend-withdrawal-callbacks) API to request CYBAVO SOFA system to resend such kind of callback(s) or through the web control panel.

<aside class="notice">
 Refer to Callback Definition , Callback Type Definition for detailed definition.
</aside>

- Please refer to the code snippet on the github project to know how to validate the callback payload.
	- [Go](https://github.com/CYBAVO/SOFA_MOCK_SERVER/blob/master/controllers/OuterController.go#L197)
	- [Java](https://github.com/CYBAVO/SOFA_MOCK_SERVER_JAVA/blob/master/src/main/java/com/cybavo/sofa/mock/MockController.java#L93)
	- [Javascript](https://github.com/CYBAVO/SOFA_MOCK_SERVER_JAVASCRIPT/blob/master/routes/wallets.js#L352)
	- [PHP](https://github.com/CYBAVO/SOFA_MOCK_SERVER_PHP/blob/master/index.php#L207)
	- [C#](https://github.com/CYBAVO/API_CHECKSUM_CALC/blob/main/c%23/checksum.cs#L89)
	- [Python](https://github.com/CYBAVO/API_CHECKSUM_CALC/blob/main/python/checksum.py#L64)

- Best practice:
	- While processing a deposit callback, in addition to verifying the checksum of the callback, use [Query Callback Detail](#query-callback-detail) API with the serial ID of the callback to perform an additional confirmation.

<a name="callback-state-change"></a>
## Callback State Change

### The state of a successful withdrawal request is changed as follows:

processing state(1) -> transaction in pool state(2) -> transaction in chain state(3) -> repeats state 3 until the confirmation count is met

### The state of a successful deposit request is changed as follows:

transaction in chain state(3) -> repeats state 3 until the confirmation count is met

<aside class="notice">
 Refer to Transaction State Definition for all transaction states definition.
</aside>


<a name="cryptocurrency-unit-conversion"></a>
# Cryptocurrency Unit Conversion

### For callback

- The amount and fees fields in the callback are in the smallest cryptocurrency unit, use `decimal` and `fee_decimal`(in the addon field) fields of callback data to convert the unit.

### For API

- Refer to decimals of [Currency Definition](#currency-definition) to convert main cryptocurrency unit.
- For the cryptocurrency token, use the token_decimals field of the [Wallet Info](#query-wallet-info) API to convert cryptocurrency token unit.


# REST API
## POST &nbsp;Create Deposit Addresses



```shell
# Request Format An example of the request: For BNB, XLM, XRP or EOS wallet:
{
  "count": 2,
  "memos": [
    "10001",
    "10002"
  ],
  "labels": [
  	"note-for-001",
  	"note-for-002"
  ]
}

curl -X POST -H "Content-Type: application/json" -d '{"count":2,"memos":["10001","10002"]}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/addresses


# An example of a successful response:
# For BNB, XLM, XRP or EOS wallet:
{
  "addresses": [
    "10001",
    "10002"
  ]
}

# For wallet excepts BNB, XLM, XRP or EOS:
{
  "addresses": [
    "0x2E7248BBCD61Ad7C33EA183A85B1856bc02C40b6",
    "0x4EB990D527c96c64eC5Bfb0D1e304840052d4975",
    "0x86434604FF857702fbE11cBFf5aC7689Af19c4Ed"
  ]
}

# For the ETH wallet that uses contract collection:
{
  "txids": [
    "0xe6dfe0d283690f636df5ea4b9df25552e6b576b88887bfb5837016cdd696e754",
    "0xdb18fd33c9a6809bfc341a1c0b2c092be5a360f394c85367f9cf316579281ab4",
    "0x18075ff1693026f93722f8b2cc0e29bf148ded5bce4dc173c8118951eceabe60",
    "0x7c6acb506ef033c09f781cc5ad6b2d0a216346758d7f955e720d6bc7a52731a5",
    "0x7da19f8c0d82cde16636da3307a6bef46eb9f398af3eb2362d230ce300509d63"
  ]
}
```

``` javascript
var request = require('request');
var options = {
  'method': 'POST',
  'url': '{{Url}}/sofa/wallets/{{wallet_id}}/addresses?t={{t}}&r={{r}}',
  'headers': {
    'Content-Type': 'application/json',
    'X-API-CODE': '{{api_code}}',
    'X-CHECKSUM': '{{checksum}}'
  },
  body: '{{postbody}}'

};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

```


Create deposit addresses on certain wallet. Once addresses are created, the CYBAVO SOFA system will callback when transactions are detected on these addresses.

##### Request

**POST** /v1/sofa/wallets/`WALLET_ID`/addresses



- `WALLET_ID` must be a deposit wallet ID
- The request includes the following parameters:
- Use [Query Deployed Contract Deposit Addresses](#query-deployed-contract-deposit-addresses) API to query deployed contract addresses.


###### Post body

| Field | Type  | Note | Description |
| :---  | :---  | :--- | :---        |
| count | int | required, max `1000` | Specify address count |
| memos | array | required (creating BNB, XLM, XRP or EOS wallet) | Specify memos for BNB, XLM, XRP or EOS deposit wallet. Refer to [Memo Requirement](#memo-requirement) |
| labels | array | optional | Specify the labels of the generated addresses or memos |

- NOTE: The length of `memos` must equal to `count` while creating addresses for BNB, XLM, XRP or EOS wallet.
- NOTE: The memos(or called destination tags) of XRP must be strings that can be converted to numbers.
- If use the `labels` to assign labels, the array length of the labels must equal to `count`.
- The label will be automatically synced to the child wallet.

##### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| addresses | array | Array of just created deposit addresses |
| txids | array | Array of transaction IDs used to deploy collection contract |


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



## GET Query Deposit Addresses

Query the deposit addresses created by the [Create Deposit Addresses](#create-deposit-addresses) API.
