---
title: CYBAVO API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - go
  - java
  - javascript


includes:
  - create-deposit-addresses
  - query-deposit-addresses
  - query-deployed-contract-deposit
  - query-pool-address
  - query-invalid-deposit-addresses
  - query-deposit-callback-detail
  - resend-deposit-collection-callbacks
  - query-deposit-wallet-balance
  - update-deposit-address-label
  - query-deposit-address-label
  - withdraw-assets
  - cancel-withdrawal-request
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
	- Refer to [Create deposit addresses](#post-nbsp-create-deposit-addresses) API
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




