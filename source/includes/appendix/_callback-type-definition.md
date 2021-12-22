## Callback Type Definition

| ID   | Description |
| :--- | :---        |
| 1 | Deposit Callback |
| 2 | Withdraw Callback |
| 3 | Collect Callback |
| 4 | Airdrop Callback |
| -1 | All callbacks (for inquiry) |


## Currency Definition

| ID   | Currency Symbol | Decimals |
| :--- | :---            | :---     |
| 0    | BTC             | 8 |
| 2    | LTC             | 8 |
| 3    | DOGE            | 8 |
| 5    | DASH            | 8 |
| 60   | ETH             | 18 |
| 144  | XRP             | 6 |
| 145  | BCH (BCHN)      | 8 |
| 148  | XLM             | 7 |
| 194  | EOS             | 4 |
| 195  | TRX             | 6 |
| 236  | BSV             | 8 |
| 354  | DOT             | 10 |
| 434  | KSM             | 12 |
| 461  | FIL             | 18 |
| 472  | AR              | 12 |
| 501  | SOL             | 9 |
| 539  | FLOW            | 8 |
| 700  | XDAI            | 18 |
| 714  | BNB             | 8 |
| 966  | MATIC           | 18 |
| 1023 | ONE             | 18 |
| 1815 | ADA             | 6 |
| 5353 | HNS             | 6 |
| 52752 | CELO           | 18 |
| 99999999988 | AVAX-C*  | 18 |
| 99999999989 | PALM*    | 18 |      
| 99999999990 | FTM*     | 18 |
| 99999999991 | OKT*     | 18 |
| 99999999992 | OPTIMISM* | 18 |
| 99999999993 | ARBITRUM* | 18 |
| 99999999994 | HECO*    | 18 |
| 99999999996 | WND*     | 12 |
| 99999999997 | BSC*     | 18 |


### Refer to [here](https://github.com/satoshilabs/slips/blob/master/slip-0044.md) for more detailed currency definitions

<aside class="notice">
The * mark represents the definition of pseudo-cryptocurrency in the CYBAVO SOFA system
</aside>


## Support Unconfirmed Balance Currency

| ID   | Currency Symbol |
| :--- | :---            |
| 0    | BTC             |
| 2    | LTC             |
| 3    | DOGE            |
| 5    | DASH            |
| 60   | ETH             |
| 145  | BCH (BCHN)      |
| 236  | BSV             |
| 700  | XDAI            |
| 966  | MATIC           |
| 1023 | ONE             |
| 52752 | CELO           |
| 99999999988 | AVAX-C*  |
| 99999999989 | PALM*    |
| 99999999990 | FTM*     |
| 99999999991 | OKT*     |
| 99999999992 | OPTIMISM* |
| 99999999993 | ARBITRUM* |
| 99999999994 | HECO*    |
| 99999999997 | BSC*     |




## Memo Requirement

| Currency | Description |
| :--- | :---        |
| XRP | Up to 32-bit unsigned integer (max 4294967295) |
| XLM | Up to 20 chars |
| EOS | Up to 256 chars |
| BNB | Up to 128 chars |


## Wallet Type Definition

| Type | Description |
| :--- | :---        |
| 0 | Vault wallet |
| 1 | Batch wallet |
| 2 | Deposit wallet |
| 3 | Withdrawal wallet |
| 5 | Deposit-withdrawal hybrid wallet |
