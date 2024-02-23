# EOS Network Resources

## Core principles

<details>
<summary><b>Network security</b></summary>

- Voter participation must not decrease
- Network must not be at increased risk

</details>

<details>
<summary><b>Code security</b></summary>

- Minimal system contract modifications
- No risk of loss of funds
- Code review should contain minimal code complexity

</details>

## Proposals

<details>
<summary><b>1.0 ✅ Modify RAM increase rate to 0</b></summary>

> [Introduction & Motivation](https://github.com/EOS-Nation/eos-network-resources/tree/main/1.%20Modify%20RAM%20Increase%20Rate)

### Proposal

Set RAM increase rate to 0 bytes per block.

- `eosio::setramrate` to `bytes_per_block=0`

### MSIG

- https://bloks.io/msig/eosnationftw/setramrate

</details>


<details>
<summary><b>1.1 🚧 Deflationary RAM rate</b></summary>

### Proposal

Allow for signed integer for `bytes_per_block` RAM rate (allows deflationary virtual RAM supply)

https://github.com/eosnetworkfoundation/eos-system-contracts/pull/101

### Requirements
- Set `setramrate::bytes_per_block` to `int16_t` (signed integer)
- Set `global2::new_ram_per_block` to `int16_t` (signed integer)

### Preconditions
- `max_ram_size` cannot be below `total_ram_bytes_reserved`

### References

- **system_contract**
  - [update_ram_supply](https://github.com/eosnetworkfoundation/eos-system-contracts/blob/c6113dbec2282825ce8d1fb6396fe82500af9019/contracts/eosio.system/src/eosio.system.cpp#L87-L103)
  - [setramrate](https://github.com/eosnetworkfoundation/eos-system-contracts/blob/c6113dbec2282825ce8d1fb6396fe82500af9019/contracts/eosio.system/src/eosio.system.cpp#L105-L110)
  - [global](https://github.com/eosnetworkfoundation/eos-system-contracts/blob/c6113dbec2282825ce8d1fb6396fe82500af9019/contracts/eosio.system/include/eosio.system/eosio.system.hpp#L142-L146)
  - [global2](https://github.com/eosnetworkfoundation/eos-system-contracts/blob/c6113dbec2282825ce8d1fb6396fe82500af9019/contracts/eosio.system/include/eosio.system/eosio.system.hpp#L168-L179)

</details>

<details>
<summary><b>1.2 🚧 Transferable RAM</b></summary>

### Proposal

New RAM system contract action to transfer RAM from one account to another without any fees.

https://github.com/eosnetworkfoundation/eos-system-contracts/pull/102

#### ACTION: `ramtransfer`

- `from {name}`
- `to {name}`
- `bytes {int64}`

### Requirements
- Charges 0% fee to transfer
- Only uncommited RAM can be transferred
- Notify `receiver` by `require_recipient`

### Preconditions
- `from` must have sufficient `ram_bytes` prior to transfer
- `from` decrease `ram_bytes` by `bytes`
- `to` must exists
- `to` account can be a contract
- `to` account can have zero available RAM bytes
- `to` increase `ram_bytes` by `bytes`
- handle `ram_managed` accounts

### References

- **system_contract**
  - [buyram](https://github.com/eosnetworkfoundation/eos-system-contracts/blob/c6113dbec2282825ce8d1fb6396fe82500af9019/contracts/eosio.system/src/delegate_bandwidth.cpp#L43-L103)
  - [sellram](https://github.com/eosnetworkfoundation/eos-system-contracts/blob/c6113dbec2282825ce8d1fb6396fe82500af9019/contracts/eosio.system/src/delegate_bandwidth.cpp#L111-L159)
- **resource_limits_manager**
  - [verify_account_ram_usage](https://github.com/AntelopeIO/leap/blob/96965434094d8d9a3808c7060061eadf5b632b8d/libraries/chain/resource_limits.cpp#L232-L242)
  - [set_account_limits](https://github.com/AntelopeIO/leap/blob/96965434094d8d9a3808c7060061eadf5b632b8d/libraries/chain/resource_limits.cpp#L249-L270)
  - [get_account_limits](https://github.com/AntelopeIO/leap/blob/96965434094d8d9a3808c7060061eadf5b632b8d/libraries/chain/resource_limits.cpp#L303-L315)
- **privileged**
  - [set_resource_limits](https://github.com/AntelopeIO/leap/blob/96965434094d8d9a3808c7060061eadf5b632b8d/libraries/chain/webassembly/privileged.cpp#L27-L35)
  - [get_resource_limits](https://github.com/AntelopeIO/leap/blob/96965434094d8d9a3808c7060061eadf5b632b8d/libraries/chain/webassembly/privileged.cpp#L37C20-L42)

</details>

<details>
<summary><b>1.3 🚧 RAM Notifications and Logging</b></summary>

### Proposal

Improve RAM logging by including additional inline actions and notifications via the use of `require_recipient`.

https://github.com/eosnetworkfoundation/eos-system-contracts/pull/103

## API Changes

- Add `require_recipient(receiver)` on `buyram` & `buyrambytes` actions

#### ACTION: `logbuyram`

- `payer {name}`
- `receiver {name}`
- `quant {asset}`
- `bytes {int64}`

</details>

<details>
<summary><b>2. ☑️ Removal of Deferred Transactions from System Contract</b></summary>

> [Introduction & Motivation](https://github.com/EOS-Nation/eos-network-resources/tree/main/2.%20Remove%20Deferred%20Transactions%20from%20System%20Contract)

### Proposal
[Deploy latest v3.2.0 system contract](https://github.com/eosnetworkfoundation/eos-system-contracts/releases/tag/v3.2.0)

- Within the system contracts the actions `system_contract::bidname`, `system_contract::buyram`, `wrap::exec` no longer issue deferred transactions.
- This is a change for the `system_contract::bidname` action, and failed bids will need an explict refund. For the `system_contract::buyram` action the default behavior remains unchanged.
- The `wrap::exec` action has been rewritten to use send instead of `send_deferred`.

</details>

<details>
<summary><s><b>3. 🚧 Staking Rewards</b></s></summary>

> [Introduction & Motivation](https://github.com/EOS-Nation/eos-network-resources/tree/main/3.%20Staking%20Rewards)

### Proposal

Revamp REX with modified parameters, increased allocation by 2% & burn system fees.

![image](https://github.com/EOS-Nation/eos-network-resources/assets/550895/3c37377c-f97f-416c-8dfa-aa241a310c34)

- Burn mechanism for system fees (Name Bids, RAM fee, PowerUp fees, more...)
  - All system fees are burned (sent to `eosio.null`)
  - Could cause the network to be deflationary
- REX to accept a portion of unallocated inflation
  - modify `producer_pay::claimrewards` to support `rex::channel_to_rex`
  - define new `rexparams` table with
    - `inflation_rex_factor=50000` (50% of unallocated inflation)
    - `num_of_maturity_buckets=5` (4 days)
  - define new `setrexparams` action to modify `inflation_rex_factor` & `num_of_maturity_buckets`
- Increase +2% of unallocated inflation going to REX
  - call `eosio::setinflation` action with the following parameters:
    - `annual_rate=500` (previously 300)
    - `inflation_pay_factor=50000` (previously 30000)
- Remove `check_voting_requirement` checks from `buyrex`
  - resolves circular dependencies between `delegatebw`, `voteproducer`, and `buyrex`. [#51](https://github.com/EOSIO/eosio.system/issues/51)
  - allows for neutral actors to participate in REX (ex: EOS EVM Bridge)

### Considerations
- Increase REX staking period
  - modify `num_of_maturity_buckets=8` to change staking period from 4 days to 7 days
- Prevent REX liquid staking
  - modify `mvtosavings` and `mvfrsavings` to be a requirement for `buyrex`
  - matured REX loans should automatically trigger `sellrex` action

### References

- [WAX Tokenomics Upgrade](https://github.com/worldwide-asset-exchange/wax-system-contracts/blob/0f83469f55098c94ab78ad2fb5b5aa268be9fc6c/tokenomics/README.md)

</details>

<details>
<summary><b>4. 🚧 Increase maximum transaction time</b></summary>

> [Introduction & Motivation](https://github.com/EOS-Nation/eos-network-resources/tree/main/4.%20Increase%20Maximum%20Transaction%20Time)

### Operations

**[Deployment of Leap 5.0.0](https://github.com/AntelopeIO/leap/releases/tag/v5.0.0-rc3) (stable release)**

- Assuming default of 30 ms for `max-transaction-time`, that effectively raises the CPU time available by 5x to 150 ms.
- Leap 5.0.0 brings the selective EOS VM OC feature which may increase some computations in EOS EVM by a similar multiplier.
- That is already getting us a significant gain in computation capacity per EOS transaction which should translate to higher overall gas limits per EVM transaction (assuming 1 EVM transaction per EOS transaction).

### No Change

- There is no need at the moment to further raise `max_transaction_cpu_usage` for the purposes of EOS EVM.

</details>

<details>
<summary><b>5. 🚧📖 PowerUp technical improvements</b></summary>

> [Introduction & Motivation](https://github.com/EOS-Nation/eos-network-resources/tree/main/5.%20PowerUp%20technical%20improvements)

### Proposal

Introduce an enhanced Powerup utility function designed to facilitate user interaction with the native Powerup action in a seamless manner.

- Make a payment using a set amount of EOS (ex: "I want to pay 1 EOS")
- Lease a specific duration of CPU time (ex: "I want 10ms of CPU")

### Requirements

- Make `powerupms` to do 1ms of CPU
- Make `powerupcost` to pay 1 EOS for CPU
- Make `powerupkb` to buy 1kb of NET

### No Change

- Powerup CPU/NET ratios remain unchanged

### References

- https://ultra.io/articles/61-3/ultra-blockchain-resource-model

</details>

<details>
<summary><b>6. 🚧📖 Cap EOS supply at 2B tokens</b></summary>

> [Introduction & Motivation](https://github.com/EOS-Nation/eos-network-resources/tree/main/6.%20Cap%20EOS%20supply%20at%202B%20tokens)

### Proposal

- Turn off inflation
- Mint ~818M EOS
- Cap EOS supply at 2B tokens
- Release newly minted tokens over period of time with aggressive release in the beginning tapering over time.

Dilute those not participating (~85% of EOS holders) rewarding those who are participating.

### Technical requirements

- Set `setinflation` to `annual_rate=0`
- Modify `eosio.token::max_supply` to `2,000,000,000.0000 EOS`
- Call `eosio.token::issue` to mint ~818M EOS
- Implement vesting contracts for newly minted tokens
- Implement long term tiered staking system
- Implement new block producer reward mechanism (after BLS is activated)

</details>

### Notes
- ✅ deployed
- ☑️ ready to deploy
- 🚧 requires development & testing
- 📖 requires additional research
