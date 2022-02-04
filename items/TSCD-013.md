# Title
Transactions to unknown/untrusted addresses 

## Description
Smart contracts often interact with other smart contracts or transfer tez to other accounts or smart contracts. These called addresses then can take over the control flow.

A called address taking over the control flow may deliberately fail when called. This may lead to DoS vulnerability for the entire smart contract solution. E.g. consider the situation of a batch payout to multiple addresses, where one address deliberately fails and causes all payouts to fail.

The called address may also call back into the caller smart contract (reentrancy). This could lead to undesirable effects and wrong states in the smart contracts. 

## Remediation
It is recommended to carefully design the smart contract and analyze the impact of initiated calls to other addresses.

If third party contracts are called, these contracts should be carefully analysed and tested before incorporating in their own smart contract design. Please also consider that the incorporated smart contract may also call other addresses (e.g. DEX calls external addresses using the Token-to-Tez payment entrypoint”).

It is not recommended to do batch payouts due to potential DoS issues. In addition to the DoS issue described here, see also “DoS with gas limits”. Thus, it is recommended to consider a picking up scheme instead of a payout scheme.

## References
- [Dexter Token Integration Checklist](https://gitlab.com/camlcase-dev/dexter/-/blob/develop/docs/token-integration.md)
- https://ligolang.org/docs/tutorials/security/security#transactions-to-untrusted-contracts
- https://ligolang.org/docs/tutorials/security/security#reentrancy-and-call-injection
- https://tezos.gitlab.io/developer/michelson_anti_patterns.html#refunding-to-a-list-of-contracts

## Samples/Test cases
