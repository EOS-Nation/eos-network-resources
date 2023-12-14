# 4. Increase Maximum Transaction Time

### Introduction

When a node receives a transaction, it begins executing the transaction. It continues until either the execution completes with a success or failure or the wall-clock time it takes to execute the transaction exceeds the maximum transaction time set on-chain.

### Motivation

The network had previously set max-transaction-time at a threshold where, often, EVM transactions were unable to complete execution before the node reached the max-transaction-time. Leap v5.0.0 increases this parameter from 30ms to 150ms, which raises the available CPU time by 5x. This boost has resulted in significant CPU capacity gains and higher overall gas limits for EVM transactions.

### Proposal
Deploy the stable version of Leap 5.0.0. This version increases the `max-transaction-time` to 150ms. More details on the new Leap version are in the release notes: https://github.com/AntelopeIO/leap/releases/tag/v5.0.0-rc2.

More details on the proposal are at https://github.com/EOS-Nation/eos-network-resources#operations .

### Timeline
Implemented, awaiting deployment
