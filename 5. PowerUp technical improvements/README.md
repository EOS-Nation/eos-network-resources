# 5. Powerup Technical Improvements

### Introduction

EOS Powerup improved the initial resource allocation system, allowing accounts to “power up” their resources temporarily. Since its release, various community initiatives have utilized this system to help improve user experience by abstracting away the resource management tools built into Antelope. 

### Motivation
While the PowerUp model is useful, it lacks a few key features that would allow it to streamline resource allocation more profoundly. Other community members have built systems that help streamline the Powerup process. However, the system native to the protocol is still complex and does not offer these features.

### Proposal
Add a new “wrapper” function that allows users to make powerup prepayments using a specific amount of EOS, lease a specific duration of CPU time, and automatically allocate CPU and NET together at a 10:1 ratio, respectively.

More details at https://github.com/EOS-Nation/eos-network-resources#proposal-3 .

### Timeline
Requires development and testing
