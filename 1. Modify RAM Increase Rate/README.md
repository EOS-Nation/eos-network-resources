# 1. Modify RAM Increase Rate

### Introduction

RAM is scarce on Antelope blockchains, and accounts can reserve RAM using the RAM market. Early in the history of the EOS blockchain, the network introduced a parameter that steadily increases the maximum amount of RAM available in the RAM market. However, this number does not affect how much RAM is on the machines Block Producers and nodes run. Currently, the maximum chain RAM in the market is over 400 GB. The current purchased RAM is around 68 GB, meaning according to the Bancor relay that runs the market, RAM is cheap. This parameter is a uint16_t, which means the RAM cannot decrease since the variable is “unsigned” and cannot be negative.

### Motivation

Modern CPUs with high single-core performance primarily target gamers rather than servers and typically have a maximum RAM of 128GB. This limit suggests that, for most block producers, the physical hardware has yet to reach a level that allows them to supply the amount of RAM the market assumes is available without switching all their infrastructure to lower-performance server CPUs.

### Proposal

Set the increasing RAM rate parameter to zero to stop the inflation of RAM supply in the RAM market.

Another suggestion is to make changes to allow a negative RAM inflation rate. This step would require additional research to mitigate any potential impacts.

View more details at https://github.com/EOS-Nation/eos-network-resources#proposal .