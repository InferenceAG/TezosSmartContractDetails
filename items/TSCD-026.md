# Title
Overspecified field annotations in calls

## Description  
It is possible to use field annotations to name the different entrypoint parameters in calls. For instance: 

CONTRACT %transfer (pair (address %from) (pair (address %to) (nat %value))) ;

This works well as long as the corresponding contract's entrypoint has no or identical field annotations. The contract casting will fail if the corresponding contract entrypoint is using different annotations like “to_” instead of “to”. The latter is defined in the TZIP-007 standard.

## Remediation
**Note: With the [Jakarta protocol](https://tezos.gitlab.io/protocols/013_jakarta.html#michelson) this is no longer required.**

It is recommended to consider the removal of the field annotations to ensure the integration of smart contracts, which are not fully compatible with the corresponding standards.

## References

## Samples/Test cases