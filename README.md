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
<summary><b>1. ✅ Modify RAM increase rate</b></summary>

### Timeline
- Immediate

### Proposal
- `eosio::setramrate` to `bytes_per_block=0`

### Considerations
- Allow for signed integer for `bytes_per_block` RAM rate (allows decreasing virtual RAM supply)
</details>

<details>
<summary><b>2. ✅ Removal of Deferred Transactions from System Contract</b></summary>

### Timeline
- Immediate

### Proposal
- Deploy latest v3.2.0 system contract
- Within the system contracts the actions `system_contract::bidname`, `system_contract::buyram`, `wrap::exec` no longer issue deferred transactions. This is a change for the `system_contract::bidname` action, and failed bids will need an explict refund. For the `system_contract::buyram` action the default behavior remains unchanged. The `wrap::exec` action has been rewritten to use send instead of `send_deferred`.

</details>

<details>
<summary><b>3. 🚧 Staking Rewards</b></summary>

### Timeline
- Requires development and testing

### Proposal

- REX to accept a portion of unallocated inflation
  - modify `producer_pay::claimrewards` to support `rex::channel_to_rex`
  - define new `global5` table with `inflation_rex_factor=20000` (previously 0)
  - define new `setrexfactor` action to modify `inflation_rex_factor`
- Increase +2% of unallocated inflation going to REX
  - call `eosio::setinflation` action with the following parameters:
    - `annual_rate=500` (previously 300)
    - `inflation_pay_factor=50000` (previously 30000)
- Remove proxy or vote for 21+ BPs requirement from REX
  - remove `check_voting_requirement` checks from `buyrex`
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

### Operations
- Deployment of Leap 5.0.0 (stable release)
  - Assuming default of 30 ms for `max-transaction-time`, that effectively raises the CPU time available by 5x to 150 ms.
  - Leap 5.0.0 brings the selective EOS VM OC feature which may increase some computations in EOS EVM by a similar multiplier.
  - That is already getting us a significant gain in computation capacity per EOS transaction which should translate to higher overall gas limits per EVM transaction (assuming 1 EVM transaction per EOS transaction).

### No Change

- There is no need at the moment to further raise `max_transaction_cpu_usage` for the purposes of EOS EVM.

</details>

<details>
<summary><b>5. 🚧 PowerUp technical improvement</b></summary>

### Timeline
- Requires development and testing

### No Change

- Powerup CPU/NET ratios remain unchanged

### Proposal
- Powerup utility smart contract actions (must be backwards compatible)
- Allow for auto-renewal
  - similar to REX's `loan_fund` when using `eosio::rentcpu` action
- Pay with fixed amount of EOS
  - similar to REX's `loan_payment` when using `eosio::rentcpu` action
  - auto-calculates net/cpu ratios based on current network usage

</details>

<details>
<summary><b>6. 🚧📖 Unified Resources</b></summary>

### Timeline
- Requires development and testing

### Proposal

- Combined CPU + NET as single ephemeral resource
  - Deprecates the requirement of NET
- Smart contract reference to allow on-chain co-signing
  - Allows dapps to pay for CPU without abuse
  - Extends WharfKit's co-signing wallet feature

</details>

### Notes
- ✅ ready to deploy
- 🚧 requires development & testing
- 📖 requires additional research