# cURL Testing Commands

<a name="curl-create-deposit-addresses"></a>
### Create Deposit Addresses

For BNB, XLM, XRP or EOS wallet:

```
curl -X POST -H "Content-Type: application/json" -d '{"count":2,"memos":["10001","10002"]}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/addresses
```

For wallet excepts BNB, XLM, XRP and EOS:

```
curl -X POST -H "Content-Type: application/json" -d '{"count":2}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/addresses
```
- [API definition](#create-deposit-addresses)

<a name="curl-query-deposit-addresses"></a>
### Query Deposit Addresses

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/addresses?start_index=0&request_number=1000
```
- [API definition](#query-deposit-addresses)

<a name="curl-query-deployed-contract-deposit-addresses"></a>
### Query Deployed Contract Deposit Addresses

```
curl 'http://localhost:8889/v1/mock/wallets/{WALLET_ID}/addresses/contract_txid?txids={TXID1},{TXID2}'
```
- [API definition](#query-deployed-contract-deposit-addresses)

<a name="curl-query-pool-address"></a>
### Query Pool Address

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/pooladdress
```
- [API definition](#query-pool-address)


<a name="curl-query-pool-address-balance"></a>
### Query Pool Address Balance

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/pooladdress/balance
```
- [API definition](#query-pool-address-balance)


<a name="curl-query-invalid-deposit-addresses"></a>
### Query Invalid Deposit Addresses

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/addresses/invalid-deposit
```
- [API definition](#query-invalid-deposit-addresses)


<a name="curl-query-deposit-callback-detail"></a>
### Query Deposit Callback Detail

```
curl 'http://localhost:8889/v1/mock/wallets/{WALLET_ID}/receiver/notifications/txid/{TX_ID}/{VOUT_INDEX}'
```
- [API definition](#query-deposit-callback-detail)


<a name="curl-resend-deposit-callbacks"></a>
### Resend Deposit/Collection Callbacks

```
curl -X POST -H "Content-Type: application/json" -d '{"notification_id":0}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/collection/notifications/manual
```
- [API definition](#resend-deposit-callbacks)


<a name="curl-query-deposit-wallet-balance"></a>
### Query Deposit Wallet Balance

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/receiver/balance
```
- [API definition](#query-deposit-wallet-balance)


<a name="curl-update-deposit-address-label"></a>
### Update Deposit Address Label

```
curl -X POST -H "Content-Type: application/json" -d '{"address":"0x2B974a3e0b491bB26e0bF146E6cDaC36EFD574a","label":"take-some-notes"}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/addresses/label
```
- [API definition](#update-deposit-address-label)


<a name="curl-query-deposit-address-label"></a>
### Query Deposit Address Label

```
curl -X POST -H "Content-Type: application/json" -d '{"addresses":["0x2B974a3De0b491bB26e0bF146E6cDaC36EFD574a","0xF401AC94D9672e79c68e56A6f822b666E5A7d644"]}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/addresses/get_labels
```
- [API definition](#query-deposit-address-label)


<a name="curl-withdraw-assets"></a>
### Withdraw Assets

```
curl -X POST -H "Content-Type: application/json" -d '{"requests":[{"order_id":"888888_1","address":"0x60589A749AAC632e9A830c8aBE042D1899d8Dd15","amount":"0.0001","memo":"memo-001","user_id":"USER01","message":"message-001"},{"order_id":"888888_2","address":"0xf16B7B8900F0d2f682e0FFe207a553F52B6C7015","amount":"0.0002","memo":"memo-002","user_id":"USER01","message":"message-002"}]}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/transactions
```
- [API definition](#withdraw-assets)


<a name="curl-cancel-withdrawal-request"></a>
### Cancel Withdrawal Request

```
curl -X POST http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/transactions/{ORDER_ID}/cancel
```
- [API definition](#cancel-withdrawal-request)


<a name="curl-query-latest-withdrawal-transaction-state"></a>
### Query Latest Withdrawal Transaction State

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/transactions/{ORDER_ID}
```
- [API definition](#query-latest-withdrawal-transaction-state)


<a name="curl-query-all-withdrawal-transaction-states"></a>
### Query All Withdrawal Transaction States

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/transactions/{ORDER_ID}/all
```
- [API definition](#query-all-withdrawal-transaction-states)


<a name="curl-query-withdrawal-transaction-event-logs"></a>
### Query Withdrawal Transaction Event Logs

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/transactions/eventlog?txid={TXID}
```
- [API definition](#query-withdrawal-transaction-event-logs)


<a name="curl-query-withdrawal-wallet-balance"></a>
### Query Withdrawal Wallet Balance

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/balance
```
- [API definition](#query-withdrawal-wallet-balance)


<a name="curl-query-withdrawal-callback-detail"></a>
### Query Withdrawal Callback Detail

```
curl 'http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/notifications/order_id/{ORDER_ID}'
```
- [API definition](#query-withdrawal-callback-detail)


<a name="curl-set-withdrawal-request-acl"></a>
### Set Withdrawal Request ACL

```
curl -X POST -H "Content-Type: application/json" -d '{"acl":"192.168.101.55"}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/transactions/acl
```
- [API definition](#set-withdrawal-request-acl)


<a name="curl-query-withdrawal-whitelist-configuration"></a>
### Query Withdrawal Whitelist Configuration

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/whitelist/config
```
- [API definition](#query-withdrawal-whitelist-configuration)


<a name="curl-query-withdrawal-wallet-transaction-history"></a>
### Query Withdrawal Wallet Transaction History

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/transactions
```
- [API definition](#query-withdrawal-wallet-transaction-history)


<a name="curl-sign-message"></a>
### Sign Message

```
curl -X POST -H "Content-Type: application/json" -d '{"message":"This is proof that I, user A, have access to this address."}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/signmessage
```
- [API definition](#sign-message)


<a name="curl-call-contract-read-abi"></a>
### Call Contract Read ABI

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/contract/read?contract=0xad6d458402f60fd3bd25163575031acdce07538d&data=0xdd62ed3e000000000000000000000000d11bd6e308b8dc1c5243d54cf41a427ca0f46943000000000000000000000000e592427a0aece92de3edee1f18e0157c05861564
```
- [API definition](#call-contract-read-abi)


<a name="curl-add-withdrawal-whitelist-entry"></a>
### Add Withdrawal Whitelist Entry

```
curl -X POST -H "Content-Type: application/json" -d '{"items":[{"address":"GCIFMEYIEWSX3K6EOPMEJ3FHW5AAPD6NW76J7LPBRAKD4JZKTISKUPHJ","memo":"85666","user_id":"USER002"}]}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/whitelist
```
- [API definition](#add-withdrawal-whitelist-entry)


<a name="curl-remove-withdrawal-whitelist-entry"></a>
### Remove Withdrawal Whitelist Entry

```
curl -X DELETE -H "Content-Type: application/json" -d '{"items":[{"address":"GCIFMEYIEWSX3K6EOPMEJ3FHW5AAPD6NW76J7LPBRAKD4JZKTISKUPHJ","memo":"85666","user_id":"USER002"}]}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/whitelist
```
- [API definition](#remove-withdrawal-whitelist-entry)


<a name="curl-check-withdrawal-whitelist"></a>
### Check Withdrawal Whitelist

```
curl -X POST -H "Content-Type: application/json" -d '{"address":"GCIFMEYIEWSX3K6EOPMEJ3FHW5AAPD6NW76J7LPBRAKD4JZKTISKUPHJ","memo":"85666","user_id":"USER002"}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/whitelist/check
```
- [API definition](#check-withdrawal-whitelist)


<a name="curl-query-withdrawal-whitelist"></a>
### Query Withdrawal Whitelist

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/whitelist
```
- [API definition](#query-withdrawal-whitelist)


<a name="curl-resend-withdrawal-callbacks"></a>
### Resend Withdrawal Callbacks

```
curl -X POST -H "Content-Type: application/json" -d '{"notification_id":0}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/sender/notifications/manual
```
- [API definition](#resend-withdrawal-callbacks)


<a name="curl-query-callback-history"></a>
### Query Callback History

```
curl 'http://localhost:8889/v1/mock/wallets/{WALLET_ID}/notifications?from_time=1561651200&to_time=1562255999&type=2'
```
- [API definition](#query-callback-history)


<a name="curl-query-callback-detail"></a>
### Query Callback Detail

```
curl -X POST -H "Content-Type: application/json" -d '{"ids":[90000000140,90000000139]}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/notifications/get_by_id
```
- [API definition](#query-callback-detail)


<a name="curl-query-wallet-synchronization-info"></a>
### Query Wallet Synchronization Info

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/blocks
```
- [API definition](#query-wallet-synchronization-info)


<a name="curl-query-transaction-average-fee"></a>
### Query Transaction Average Fee

```
curl -X POST -H "Content-Type: application/json" -d '{"block_num":1}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/autofee
```
- [API definition](#query-transaction-average-fee)


<a name="curl-batch-query-transaction-average-fees"></a>
### Batch Query Transaction Average Fees

```
curl -X POST -H "Content-Type: application/json" -d '{"block_nums":[1,5,10]}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/autofees
```
- [API definition](#batch-query-transaction-average-fees)


<a name="curl-query-vault-wallet-transaction-history"></a>
### Query Vault Wallet Transaction History

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/transactions?start_index=0&from_time=1559664000&to_time=1562255999&request_number=8
```
- [API definition](#query-vault-wallet-transaction-history)

<a name="curl-query-vault-wallet-balance"></a>
### Query Vault Wallet Balance

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/vault/balance
```
- [API definition](#query-vault-wallet-balance)


<a name="curl-activate-api-code"></a>
### Activate API Code

```
curl -X POST http://localhost:8889/v1/mock/wallets/{WALLET_ID}/apisecret/activate
```
- [API definition](#activate-api-code)


<a name="curl-query-api-code-status"></a>
### Query API Code Status

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/apisecret
```
- [API definition](#query-api-code-status)


<a name="curl-refresh-api-code"></a>
### Refresh API Code

```
curl -X POST -H "Content-Type: application/json" -d '{"refresh_code":"3EbaSPUpKzHJ9wYgYZqy6W4g43NT365bm9vtTfYhMPra"}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/refreshsecret
```
- [API definition](#query-api-code-status)


<a name="curl-query-wallet-info"></a>
### Query Wallet Info

```
curl http://localhost:8889/v1/mock/wallets/{WALLET_ID}/info
```
- [API definition](#query-wallet-info)


<a name="curl-verify-addresses"></a>
### Verify Addresses

```
curl -X POST -H "Content-Type: application/json" -d '{"addresses":["0x635B4764D1939DfAcD3a8014726159abC277BecC","1CK6KHY6MHgYvmRQ4PAafKYDrg1ejbH1cE"]}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/addresses/verify
```
- [API definition](#verify-addresses)


<a name="curl-inspect-callback-endpoint"></a>
### Inspect Callback Endpoint

```
curl -X POST -H "Content-Type: application/json" -d '{"test_number":102999}' \
http://localhost:8889/v1/mock/wallets/896541/notifications/inspect
```
- [API definition](#inspect-callback-endpoint)


<a name="curl-list-wallets"></a>
### List Wallets

```
curl http://localhost:8889/v1/mock/wallets/readonly/walletlist
```
- [API definition](#list-wallets)


<a name="curl-query-wallets-balance"></a>
### Query Wallets Balance

```
curl http://localhost:8889/v1/mock/wallets/readonly/walletlist/balances
```
- [API definition](#query-wallets-balance)


##### [Back to top](#table-of-contents)

<a name="other-language-versions"></a>
# Other Language Versions
- [Java](https://github.com/CYBAVO/SOFA_MOCK_SERVER_JAVA)
- [Javascript](https://github.com/CYBAVO/SOFA_MOCK_SERVER_JAVASCRIPT)
- [PHP](https://github.com/CYBAVO/SOFA_MOCK_SERVER_PHP)
