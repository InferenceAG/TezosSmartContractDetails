# Title
Improper access controls

## Description
Functions are exposed as entrypoints, but lack appropriate access controls. This can lead to a vulnerability, if a malicious user is able to make unauthorized or unintended state changes in the smart contract.

## Remediation
It is recommended to implement effective access controls preventing unauthorized execution of entrypoints.

## References
- https://ligolang.org/docs/tutorials/security/security#incorrect-authorisation-checks

##  Samples/Test cases
