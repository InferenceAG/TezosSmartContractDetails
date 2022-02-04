# Title
Missing signature replay protection

## Description
Smart contracts sometimes have to verify signed data (e.g. off-chain signing or verification of signatures from multisig contracts). This could lead to a vulnerability if there is no replay protection implemented. The signed data could be submitted again to the same contract or to other, equal types of contracts.

## Remediation
It is recommended to include in the signed data also a nonce/counter value and the contract address.

## References
- https://tezos.gitlab.io/developer/michelson_anti_patterns.html#signatures-alone-do-not-prevent-replay-attacks

## Samples/Test cases
