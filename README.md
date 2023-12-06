# EOS Network Resources

## Overview

<details>
<summary><b>1. Modify RAM increase rate</b></summary>

### Proposal
- `eosio::setramrate` to `bytes_per_block=0`

### Roadmap
- Allow for signed integer for `bytes_per_block` RAM rate (allows decreasing virtual RAM supply)
</details>

<details>
<summary><b>2. Increase CPU block size</b></summary>

### Proposal
- Increase `max_block_cpu_usage` (requirement for EOS EVM transactions)
- Increase `max_inline_action_depth=32` (allows for more inline actions)

</details>

<details>
<summary><b>3. Staking Rewards</b></summary>

### Proposal

- Modify `producer_pay::claimrewards` to support `rex::channel_to_rex` to accept a portion of unallocated inflation
- Set `eosio::setinflation` to the following parameters:
  - `annual_rate=500`
  - `inflation_pay_factor=50000`
- Remove `check_voting_requirement` checks from `buyrex`
  - removes proxy or vote for 21+ BPs requirement
  - allows for neutral actors to participate in REX (ex: EOS EVM Bridge)

### Roadmap

- Improvements to `mvtosavings` and `mvfrsavings` to be a requirement for `buyrex`
- Increase `num_of_maturity_buckets` to 28

</details>

<details>
<summary><b>4. PowerUp technical improvement</b></summary>

### Proposal
- Powerup utility smart contract actions (must be backwards compatible)
    - Allow for auto-renewal (similar to how REX had renewals)
    - Pay with fixed amount of EOS (instead of calculating net/cpu ratios)
- No change in powerup CPU/NET ratios

</details>

<details>
<summary><b>5. Unified Resources</b></summary>

### Proposal

- Combined CPU + NET as single ephemeral resource
  - Deprecates the requirement of NET
- Smart contract reference to allow on-chain co-signing
  - Allows dapps to pay for CPU without abuse
  - Extends WharfKit's co-signing wallet feature

</details>
