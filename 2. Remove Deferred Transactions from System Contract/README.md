# 2. Remove Deferred Transactions from System Contract

### Introduction
Deferred transactions are additional actions that a transaction scheduled for a later time. Deferred transactions may fail. Unlike inline actions, a deferred transaction failure doesn’t revert the original action, leading to potential edge cases that cause numerous development headaches.

### Motivation
Developers have agreed that using deferred transactions is a bad practice due to the unreliability and the added logic that contracts need to implement deferred transactions safely. Thus, deferred transactions were deprecated long ago, and Leap v5.0.0 removes them. In light of this, the system contracts currently using deferred transactions should be refactored to avoid using deferred transactions.

### Proposal
The Network should deploy the latest system contract, v3.2.0, which removes deferred transactions from the bidname, buyram, and wrap::exec actions. The primary impact on users is that for the bidname action, failed name bids will need to request an explicit refund.

More details at https://github.com/EOS-Nation/eos-network-resources#proposal-1 .
