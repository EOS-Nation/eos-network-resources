# 3. Staking Rewards

### Introduction

The REX resource exchange provides liquidity to EOS resources, increasing the efficiency of resource allocation and offering a small native yield to EOS users willing to stake their tokens for rental. The system, while robust, is currently underutilized, with 58M EOS staked in the rental exchange (~0.5% of total supply). The low rewards for staking to REX and the existence of other higher-yield staking opportunities keep most tokens out of the resource exchange.

### Motivation

REX APY is small, at under 0.2% annual yield. This low yield makes it difficult to incentivize users to stake their tokens in REX, as more lucrative yield opportunities exist. Much in the same way as the Yield+ program incentivized increased DeFi TVL on EOS, the network could dramatically increase REX rewards and incentivize REX usage by directing a portion of token issuance to REX, offering a native staking solution that could be competitive with other EOS Yield offerings.

Precise APY would depend on usage. REX rewards are split pro-rata among all REX users, so low usage would result in high yields, which would decrease with increased usage to a minimum of 2% yield with 100% of tokens staked to REX and no other fees generated.

Because REX requires users of the system to vote on at least 21 block producers, this removes the option of REX staking for exchanges and other entities that, for legal or other reasons, do not wish to vote. It also prevents users of EOS EVM or other staking smart contracts from receiving any REX rewards.

### Proposal

Increase token issuance by 2% and modify the claimrewards action to support the ability to channel these new tokens to REX. Create a new table that tracks the portion of inflation sent to REX and the number of maturity buckets (the number of days REX funds remain locked after deposit + 1). Create an action to modify that table.

Remove voting requirements from REX to allow REX staking from EOS EVM or other staking smart contracts. Removing these requirements also resolves circular dependencies requiring users to stake tokens to NET or CPU in order to stake tokens to REX, as outlined in https://github.com/EOSIO/eosio.system/issues/51 .

Additional considerations include increasing the staking lockup period and modifying the REX contract to prevent liquid REX staking.

More details at https://github.com/EOS-Nation/eos-network-resources#proposal-2 .

### Timeline

Requires development and testing

### Code references

```c++
/**
* @brief Channels system fees to REX pool
*
* @param from - account from which asset is transferred to REX pool
* @param amount - amount of tokens to be transferred
* @param required - if true, asserts when the system is not configured to channel fees into REX
*/
void system_contract::channel_to_rex( const name& from, const asset& amount, bool required )
{
    if ( rex_available() ) {
        add_to_rex_return_pool( amount );
        // inline transfer to rex_account
        token::transfer_action transfer_act{ token_account, { from, active_permission } };
        transfer_act.send( from, rex_account, amount, std::string("transfer from ") + from.to_string() + " to eosio.rex" );
        return;
    }
}
```

```c++
/**
 * @brief Calculates maturity time of purchased REX tokens which is 4 days from end
 * of the day UTC
 *
 * @return time_point_sec
 */
time_point_sec system_contract::get_rex_maturity()
{
    const uint32_t num_of_maturity_buckets = 5;
    static const uint32_t now = current_time_point().sec_since_epoch();
    static const uint32_t r   = now % seconds_per_day;
    static const time_point_sec rms{ now - r + num_of_maturity_buckets * seconds_per_day };
    return rms;
}
```

### ACTION `eosio::setinflation`
- `annual_rate`: Annual inflation rate of the core token supply.
- `inflation_pay_factor`: Inverse of the fraction of the inflation used to reward block producers.
- `votepay_factor`: Inverse of the fraction of the block producer rewards to be distributed proportional to blocks produced.

```json
{
  "annual_rate": 300,
  "inflation_pay_factor": 30000,
  "votepay_factor": 40000
}
```

### TABLE `eosio::global4` changes

- `annual_rate`: 300 => 500
- `inflation_pay_factor`: 30000 => 50000
- `votepay_factor`: N/A

### TABLE `eosio::global5` new

- `num_of_maturity_buckets`: 5
- `inflation_rex_factor`: 20000