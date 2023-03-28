# Title 
DoS with gas limits

## Description
Gas is used to bill for the execution of smart contracts. The Tezos blockchain knows a gas limit per operation and a gas limit per block. If one of these gas limits is reached, the execution fails. This may lead to a vulnerability to a constantly failing smart contract, since the gas limit is reached already when loading and deserializing the contract at the beginning or when a specific part of a contract is executed.

Inappropriate use of storage by smart contracts or the possibility that smart contract users can provide their own data may lead to a vulnerability where the entire contact or part of the contract when executed fails.

Some examples:
Smart contract appends data continuously in storage. In case the data is stored in non-lazily deserialized storage types, the contract fails every time and becomes unusable as soon as the gas limits are exhausted by loading & deserializing the contract’s storage. In case the content is stored in lazily deserialized storage types, the contract will fail as soon as the data is loaded.
Smart contract users are able to store content in the smart contract. Due to missing checks and limitations in the smart contract code, a user may store a very big number or string in the storage. Thus, the contract fails every time in the case that this large content is stored in non-lazily deserialized storage types. The contract also fails as soon as the corresponding content is loaded from lazily deserialized storage types.
Iterations through elements of corresponding data structure types (e.g. list or maps) could exhaust gas before all elements are processed.

## Remediation
It is recommended to carefully design contract storage, iterations, and validation checks if user input is stored to prevent DoS gas limits attacks.

## References
- https://tezos.gitlab.io/developer/michelson_anti_patterns.html#avoid-batch-operations-when-users-can-increase-the-size-of-the-batch
- https://ligolang.org/docs/tutorials/security/?lang=pascaligo#resource-constraints

## Samples/Test cases
