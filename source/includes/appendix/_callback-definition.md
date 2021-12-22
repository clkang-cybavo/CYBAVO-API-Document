# Appendix
## Callback Definition

``` shell
# Callback sample:
# If the `state` of callback is 5 (Failed), the detailed failure reason will put in `addon` field (key is `err_reason`). See the callback sample bellow.
{
  "type": 1,
  "serial": 90000000619,
  "order_id": "",
  "currency": "ETH",
  "txid": "0xc99a4941f87364c9679fe834f99bc12cbacfc577dedf4f34c4fd8833a68a0b00",
  "block_height": 8336269,
  "tindex": 43,
  "vout_index": 0,
  "amount": "500000000000000000",
  "fees": "945000000000000",
  "memo": "",
  "broadcast_at": 1595296751,
  "chain_at": 1595296751,
  "from_address": "0x8382Cc1B05649AfBe179e341179fa869C2A9862b",
  "to_address": "0x32d638773cB85965422b3B98e9312Fc9392307BC",
  "wallet_id": 5,
  "state": 3,
  "confirm_blocks": 2,
  "processing_state": 2,
  "addon": {
    "fee_decimal": 18
  },
  "decimal": 18,
  "currency_bip44": 60,
  "token_address": ""
}

# Callback with state 5 (Failed) sample:
{
  "type": 2,
  "serial": 20000000155,
  "order_id": "1_69",
  "currency": "ETH",
  "txid": "",
  "block_height": 0,
  "tindex": 0,
  "vout_index": 0,
  "amount": "1000000000000000",
  "fees": "",
  "memo": "",
  "broadcast_at": 0,
  "chain_at": 0,
  "from_address": "",
  "to_address": "0x60589A749AAC632e9A830c8aBE041899d8Dd15",
  "wallet_id": 2,
  "state": 5,
  "confirm_blocks": 0,
  "processing_state": 0,
  "addon": {
    "err_reason": "Illegal Transaction Format: To 0x60589A749AAC632e9A830c8aBE041899d8Dd15"
  },
  "decimal": 18,
  "currency_bip44": 60,
  "token_address": ""
}


# Deposit callback with blocklist_tags sample:
{
  "type": 1,
  "serial": 90000002797,
  "order_id": "",
  "currency": "ETH",
  "txid": "0xbb38e22c33cbc33ad5a58a88bfee0905968062fe34e33eb6e28861771686cf45",
  "block_height": 11075566,
  "tindex": 7,
  "vout_index": 0,
  "amount": "10000000000000000",
  "fees": "31500000315000",
  "memo": "",
  "broadcast_at": 1632195931,
  "chain_at": 1632195931,
  "from_address": "0xD5909BacFc1faD78e4e45E9e2feF8b4e61c8Fd0d",
  "to_address": "0x319b269ef02AB7e6660f7e6cb181D0CD06E2E4a0",
  "wallet_id": 854512,
  "processing_state": 2,
  "addon": {
    "address_label": "",
    "aml_screen_pass": false,
    "aml_tags": {
      "cybavo": {
        "score": 100,
        "tags": [
          "TEST"
        ],
        "blocked": true
      }
    },
    "blocklist_tags": [
      "cybavo(100): TEST"
    ],
    "fee_decimal": 18
  },
  "decimal": 18,
  "currency_bip44": 60,
  "token_address": ""
}
```

<table>
  <tr>
    <td>Field</td>
    <td>Type</td>
    <td>Description</td>
  </tr>
  <tr>
    <td>type</td>
    <td>int</td>
    <td>
   	 	<table>
   	 	  <thead><tr><td>ID</td><td>Type</td></tr></thead>
   	 	  <tbody>
		    <tr><td>1</td><td>Deposit</td></tr>
  		    <tr><td>2</td><td>Withdraw</td></tr>
  		    <tr><td>3</td><td>Collect</td></tr>
  		    <tr><td>4</td><td>Airdrop</td></tr>
   	 	  </tbody>
		</table>
    </td>
  </tr>
  <tr>
    <td>serial</td>
    <td>int</td>
    <td>The unique serial of callback</td>
  </tr>
  <tr>
    <td>order_id</td>
    <td>string</td>
    <td>The unique order ID of withdrawal request</td>
  </tr>
  <tr>
    <td>currency</td>
    <td>string</td>
    <td>Cryptocurrency of the callback<br>This field is for human reading only and may change in the future. Do not use this string as currency definition, use the fields <b>currency_bip44</b> and <b>token_address</b> as currency definition.</td>
  </tr>
  <tr>
    <td>txid</td>
    <td>string</td>
    <td>Transaction identifier</td>
  </tr>
  <tr>
    <td>block_height</td>
    <td>int64</td>
    <td>The block height show the transaction was packed in which block</td>
  </tr>
  <tr>
    <td>tindex</td>
    <td>int</td>
    <td>The index of transaction in its block</td>
  </tr>
  <tr>
    <td>vout_index</td>
    <td>int</td>
    <td>The index of vout in its transaction</td>
  </tr>
  <tr>
    <td>amount</td>
    <td>string</td>
    <td>Transaction amount denominated in the smallest cryptocurrency unit</td>
  </tr>
  <tr>
    <td>fees</td>
    <td>string</td>
    <td>Mining fee denominated in the smallest cryptocurrency unit</td>
  </tr>
  <tr>
    <td>broadcast_at</td>
    <td>int64</td>
    <td>When to broadcast the transaction in UTC time</td>
  </tr>
  <tr>
    <td>chain_at</td>
    <td>int64</td>
    <td>When was the transaction packed into block (in chain) in UTC time</td>
  </tr>
  <tr>
    <td>from_address</td>
    <td>string</td>
    <td>The source address of the transaction</td>
  </tr>
  <tr>
    <td>to_address</td>
    <td>string</td>
    <td>The destination address of the transaction</td>
  </tr>
  <tr>
    <td>wallet_id</td>
    <td>int64</td>
    <td>The wallet ID of the callback</td>
  </tr>
  <tr>
    <td>state</td>
    <td>int</td>
    <td>
      Possible states (listed in the Transaction State Definition table)
	 	<table>
	 	  <thead><tr><td>ID</td><td>Description</td></tr></thead>
	 	  <tbody>
		    <tr><td>1</td><td>Processing</td></tr>
		    <tr><td>2</td><td>TXID in pool</td></tr>
		    <tr><td>3</td><td>TXID in chain</td></tr>
		    <tr><td>5</td><td>Failed (the err_reason of addon field will contain detailed error reason)</td></tr>
		    <tr><td>8</td><td>Cancelled</td></tr>
		    <tr><td>10</td><td>Dropped</td></tr>
		    <tr><td>11</td><td>Transaction Failed</td></tr>
	 	  </tbody>
		</table>
    </td>
  </tr>
  <tr>
    <td>confirm_blocks</td>
    <td>int64</td>
    <td>Number of confirmations</td>
  </tr>
  <tr>
    <td>processing_state</td>
    <td>int</td>
    <td>
	 	<table>
	 	  <thead><tr><td>ID</td><td>Description</td></tr></thead>
	 	  <tbody>
		    <tr><td>-1</td><td>If the state is 5(failed), 8(cacelled), 10(dropped) or 11(transaction failed)</td></tr>
		    <tr><td>0</td><td>In fullnode mempool</td></tr>
		    <tr><td>1</td><td>In chain (the transaction is already on the blockchain but the confirmations have not been met)</td></tr>
		    <tr><td>2</td><td>Done (the transaction is already on the blockchain and satisfy confirmations)</td></tr>
	 	  </tbody>
		</table>
    </td>
  </tr>
  <tr>
    <td>addon</td>
    <td>key-value pairs</td>
    <td>
    The extra information of this callback
	 	<table>
	 	  <thead><tr><td>Key</td><td>Value (Description)</td></tr></thead>
	 	  <tbody>
		    <tr><td>err_reason</td><td>Will contain detail error reason if state is 5(Failed)</td></tr>
		    <tr><td>fee_decimal</td><td>The decimal of cryptocurrency miner fee</td></tr>
		    <tr><td>blocklist_tags</td><td>The tags of CYBAVO AML detection</td></tr>
		    <tr><td>address_label</td><td>The label of the deposit address</td></tr>
		    <tr><td>contract_abi</td><td>The contract ABI of the withdrawal request</td></tr>
		    <tr><td>token_id</td><td>Transferred token ID</td></tr>
		    <tr><td>aml_tags</td><td>Detailed CYBAVO AML detection information includes score, tags and blocked flag</td></tr>
		    <tr><td>aml_screen_pass</td><td>Pass or fail CYBAVO AML screening</td></tr>
	 	  </tbody>
		</table>
    </td>
  </tr>
  <tr>
    <td>decimal</td>
    <td>int</td>
    <td>The decimal of cryptocurrency</td>
  </tr>
  <tr>
    <td>currency_bip44</td>
    <td>int64</td>
    <td>
   	 	<table>
   	 	  <thead><tr><td>ID</td><td>Currency Symbol</td><td>Decimals</td></tr></thead>
   	 	  <tbody>
		    <tr><td>0</td><td>BTC</td><td>8</td></tr>
  		    <tr><td>2</td><td>LTC</td><td>8</td></tr>
  		    <tr><td>3</td><td>DOGE</td><td>8</td></tr>
  		    <tr><td>5</td><td>DASH</td><td>8</td></tr>
  		    <tr><td>60</td><td>ETH</td><td>18</td></tr>
  		    <tr><td>144</td><td>XRP</td><td>6</td></tr>
  		    <tr><td>145</td><td>BCH (BCHN)</td><td>8</td></tr>
  		    <tr><td>148</td><td>XLM</td><td>7</td></tr>
  		    <tr><td>194</td><td>EOS</td><td>4</td></tr>
   		    <tr><td>195</td><td>TRX</td><td>6</td></tr>
   		    <tr><td>236</td><td>BSV</td><td>8</td></tr>
   		    <tr><td>354</td><td>DOT</td><td>10</td></tr>
   		    <tr><td>434</td><td>KSM</td><td>12</td></tr>
   		    <tr><td>461</td><td>FIL</td><td>18</td></tr>
   		    <tr><td>472</td><td>AR</td><td>12</td></tr>
   		    <tr><td>501</td><td>SOL</td><td>18</td></tr>
   		    <tr><td>539</td><td>FLOW</td><td>8</td></tr>
   		    <tr><td>700</td><td>XDAI</td><td>8</td></tr>
   		    <tr><td>714</td><td>BNB</td><td>8</td></tr>
   		    <tr><td>966</td><td>MATIC</td><td>8</td></tr>
   		    <tr><td>1023</td><td>ONE</td><td>6</td></tr>
   		    <tr><td>1815</td><td>ADA</td><td>6</td></tr>
   		    <tr><td>5353</td><td>HNS</td><td>6</td></tr>
   		    <tr><td>52752</td><td>CELO</td><td>18</td></tr>
   		    <tr><td>99999999988</td><td>AVAX-C*</td><td>18</td></tr>
   		    <tr><td>99999999989</td><td>PALM*</td><td>18</td></tr>
   		    <tr><td>99999999990</td><td>FTM*</td><td>18</td></tr>
   		    <tr><td>99999999991</td><td>OKT*</td><td>12</td></tr>
   		    <tr><td>99999999992</td><td>OPTIMISM*</td><td>12</td></tr>
   		    <tr><td>99999999993</td><td>ARBITRUM*</td><td>12</td></tr>
   		    <tr><td>99999999994</td><td>HECO*</td><td>12</td></tr>
   		    <tr><td>99999999996</td><td>WND*</td><td>12</td></tr>
   		    <tr><td>99999999997</td><td>BSC*</td><td>18</td></tr>
  	 	  </tbody>
		</table>
		*pseudo cryptocurrency definition in the CYBAVO SOFA system
    </td>
  </tr>
  <tr>
    <td>token_address</td>
    <td>string</td>
    <td>The contract address of cryptocurrency</td>
  </tr>
  <tr>
    <td>memo</td>
    <td>string</td>
    <td>The memo/destination tag of the transaction</td>
  </tr>
</table>

--- 

### Transaction State Definition

| ID   | State | Description | Callback | Callback Type |
| :-:  | :---  | :--- | :-: | :-- |
| 0    | Init        | The withdrawal request has been enqueued in the CYBAVO SOFA system | - | - |
| 1    | Processing  | The withdrawal request is processing in the CYBAVO SOFA system | O | Withdrawal(2) |
| 2    | TXID in pool | The withdrawal transaction is pending in the fullnode mempool | O | Withdrawal(2) |
| 3    | TXID in chain | The transaction is already on the blockchain | O | Deposit(1), Withdrawal(2), Collect(3) |
| 4    | TXID confirmed in N blocks | The withdrawal transaction is already on the blockchain and satisfy confirmations | - | - |
| 5    | Failed | Fail to create the withdrawal transaction | O | Withdrawal(2) |
| 6    | Resent | The transaction has been successfully resent | - | - |
| 7    | Blocked due to risk controlled | The deposit or withdrawal transaction was temporarily blocked due to a violation of the risk control rules | - | - |
| 8    | Cancelled | The withdrawal request is cancelled via web console | O | Withdrawal(2) |
| 9    | UTXO temporarily not available | The withdrawal request has been set as pending due to no available UTXO | - | - |
| 10   | Dropped | A long-awaited withdrawal transaction that does not appear in the memory pool of the fullnode will be regarded as dropped  | O | Withdrawal(2) |
| 11   | Transaction Failed | The withdrawal transaction is regarded as a failed transaction by the fullnode | O | Withdrawal(2) |
| 12   | Paused | The withdrawal request has been paused | - | - |

