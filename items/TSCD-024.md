# Title
Use of block level or block timestamp for time

## Description
Contracts often need time values for certain functionality. Developers are using [block timestamp](https://tezos.gitlab.io/michelson-reference/#instr-NOW) and [block level](https://tezos.gitlab.io/michelson-reference/#instr-LEVEL) to get an approximation to the actual time resp. a time delta.

### Block timestamp
Please see [Timestamps](https://ligolang.org/docs/tutorials/security/security#timestamps)

### Block level
Blocks (priority 0 blocks) are created about every 30 seconds with the current protocol Hangzhou. Thus, smart contract developer may use this 30s block creation time and block level to approximate time.

However, this is just a rough approximation and can change (significant) for different reasons:
- a protocol upgrade may change the block creation time (e.g. Granada introduced 30s block creation time instead of 60s)
- blocks are created with higher priority than 0 priority
- block creation may gets temporarily delayed / halted (e.g. due to an upgrade as it happened with Granada to Hangzhou)

## Remediation
Smart contracts developers should be aware that using these values are just an approximation to the current time and that innappropriate use may lead to unexcpeted effects or vulnerabilities. 

## References
- https://ligolang.org/docs/tutorials/security/security#timestamps

## Samples/Test cases
