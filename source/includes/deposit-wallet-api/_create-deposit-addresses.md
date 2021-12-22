# Deposit Wallet API
## POST / Create Deposit Addresses



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
const crypto = require('crypto');
const api_server_url = "https://sandtest.api.com";  // replace with api url
const apikey = '4x1---xxxx-----vjK' // replace with your api key
const secret = 'xxxxxxxxxxxxxxxxxxxxxxxxx'; // replace with your api secret
var walletID = `379988`; // replace with your wallet ID
var method = `POST`;
var api_route = `/v1/sofa/wallets/${walletID}/addresses`;
var params = null;
var postData = `{
  "count": 2,
  "memos": [
    "10001",
    "10002"
  ],
  "labels": [
    "note-for-001",
    "note-for-002"
  ]
}`;

const randomString = (length) => {
  let r = '';
  const charset = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
  for (let i = 0; i < length; i++)
    r += charset.charAt(Math.floor(Math.random() * charset.length));
  return r;
}

const tryParseJSON = (s) => {
  try {
    const o = JSON.parse(s);
    if (o && typeof o === 'object') {
      return o;
    }
  } catch (e) {}
  return s;
}

const buildChecksum = (params, secret, t, r, postData) => {
    const p = params || [];
    p.push(`t=${t}`, `r=${r}`);
    if (!!postData) {
      if (typeof postData === 'string') {
        p.push(postData);
      } else {
        p.push(JSON.stringify(postData));
      }
    }
    p.sort();
    p.push(`secret=${secret}`);
    return crypto.createHash('sha256').update(p.join('&')).digest('hex');
}

const doRequest = (url, options, postData) => {
  console.log('request -> ', url, ', options ->', options);
  return new Promise((resolve, reject) => {
    let req = https.request(url, options, (res) => {
      let resData = [];
      res.on('data', (fragments) => {
        resData.push(fragments);
      });
      res.on('end', () => {
        let resBody = Buffer.concat(resData);
        resolve({ result: tryParseJSON(resBody.toString()), statusCode: res.statusCode });
      });
      res.on('error', (error) => {
        reject(error);
      });
    });
    req.on('error', (error) => {
      reject(error);
    });
    if (!!postData) {
      if (options.method === 'DELETE') {
        req.useChunkedEncodingByDefault = true;
      }
      req.write(postData);
    }
    req.end();
  });
}

const makeRequest = async (targetID, method, api, params, postData) => {
    if (targetID < 0 || method === '' || api === '') {
      return { error: 'invalid parameters' };
    }
    const r = randomString(8);
    const t = Math.floor(Date.now()/1000);
    let url = `${api_server_url}${api}?t=${t}&r=${r}`;
    if (!!params) {
      url += `&${params.join('&')}`;
    }
    const apiCodeObj = {code:apikey,secret:secret}

    const options = {
      method,
      headers: {
        'X-API-CODE': apiCodeObj.code,
        'X-CHECKSUM': buildChecksum(params, apiCodeObj.secret, t, r, postData),
        "User-Agent": "nodejs",
      },
    };
  
    if (method === 'POST' || method === 'DELETE') {
      options.headers['Content-Type'] = 'application/json';
    }
  
    try {
      let result = await doRequest(url, options, postData);
      const resp = tryParseJSON(result);
      console.log('response ->', resp ? JSON.stringify(resp) : '');
      return resp;
    } catch(error) {
      const resp = tryParseJSON(error);
      console.log('response ->', resp ? JSON.stringify(resp) : '');
      return resp;
    }
}

const call = async() => {
    await makeRequest(walletID,method,api_route,params,postData);
}
call();

```

``` go

```

``` java

```

Create deposit addresses on certain wallet. Once addresses are created, the CYBAVO SOFA system will callback when transactions are detected on these addresses.

### Request

**POST** /v1/sofa/wallets/`WALLET_ID`/addresses


<aside class="notice">
`WALLET_ID` must be a deposit wallet ID
</aside>

### Use [Query Deployed Contract Deposit Addresses](#get-nbsp-query-deposit-addresses) API to query deployed contract addresses.

---

### Post body

The request includes the following parameters:

| Field | Type  | Note | Description |
| :---  | :---  | :--- | :---        |
| count | int | required, max `1000` | Specify address count |
| memos | array | required (creating BNB, XLM, XRP or EOS wallet) | Specify memos for BNB, XLM, XRP or EOS deposit wallet. Refer to [Memo Requirement](#memo-requirement) |
| labels | array | optional | Specify the labels of the generated addresses or memos |




- The length of `memos` must equal to `count` while creating addresses for BNB, XLM, XRP or EOS wallet.

- The memos(or called destination tags) of XRP must be strings that can be converted to numbers.


- If use the `labels` to assign labels, the array length of the labels must equal to `count`.The label will be automatically synced to the child wallet.


### Response Format

The response includes the following parameters:

| Field | Type  | Description |
| :---  | :---  | :---        |
| addresses | array | Array of just created deposit addresses |
| txids | array | Array of transaction IDs used to deploy collection contract |


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


