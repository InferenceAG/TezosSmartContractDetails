# Title
Entrypoint visibility

## Description 
Functions are erroneously exposed as entrypoints. This can lead to a vulnerability, if a malicious user is able to make unauthorized or unintended state changes in the smart contract.

## Remediation 
It is recommended that only functions are exposed as entrypoints which are required and have appropriate access controls in place. See also TSCW-004.

## References


## Samples/Test cases
