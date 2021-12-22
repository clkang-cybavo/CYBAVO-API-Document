# Testing

## How to compile Mock Server


``` shell
# Configure CYBAVO API server URL in mockserver.app.conf
api_server_url="BACKEND_SERVER_URL"

# Put wallet API code/secret into mock server
curl -X POST -H "Content-Type: application/json" -d '{"api_code":"API_CODE","api_secret":"API_SECRET"}' \
http://localhost:8889/v1/mock/wallets/{WALLET_ID}/apitoken

# Notification Callback URL
http://localhost:8889/v1/mock/wallets/callback

# Withdrawal Authentication Callback URL
http://localhost:8889/v1/mock/wallets/withdrawal/callback


```

- Put sample code to {YOUR\_GO\_PATH}/github.com/cybavo/SOFA\_MOCK\_SERVER
- Execute
	- glide install
	- go build ./mockserver.go
	- ./mockserver

### Setup configuration


### Put wallet API code/secret into mock server
-	Get API code/secret on web control panel
	-	API_CODE, API\_SECRET, WALLET\_ID
- 	Put API code/secret to mock server's databaså

### Register mock server callback URL
Operate on web control panel


<aside class="notice">
  The withdrawal authentication callback URL once set, every withdrawal request will callback this URL to get authentication to proceed withdrawal request.
</aside>

### Refer to [WithdrawalCallback()](https://github.com/CYBAVO/SOFA_MOCK_SERVER/blob/master/controllers/OuterController.go#L183) function in mock server OuterController.go


## Other Language Versions
- [Java](https://github.com/CYBAVO/SOFA_MOCK_SERVER_JAVA)
- [Javascript](https://github.com/CYBAVO/SOFA_MOCK_SERVER_JAVASCRIPT)
- [PHP](https://github.com/CYBAVO/SOFA_MOCK_SERVER_PHP)