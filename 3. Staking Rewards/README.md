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

### TABLE eosio::global4 changes

- `annual_rate`: 300 => 500
- `inflation_pay_factor`: 30000 => 50000
- `votepay_factor`: N/A