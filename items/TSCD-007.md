# Title
Use of block values for randomness

## Description
NOW provides the timestamp of the current block and is set within certain limits by the baker. Thus, bakers can influence the timestamp and the timestamp is predictable.  

## Remediation
It is recommended to not make use of any block values as a source of randomness. Make use of trusted oracles or commitment schemes.

## References
- https://ligolang.org/docs/tutorials/security/?lang=pascaligo#timestamps

## Samples/Test cases
