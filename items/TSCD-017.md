# Title
Transaction ordering

## Description
Transactions can be observed before they are baked into a block. This may lead to a vulnerability where these observers try to include an own transaction before to benefit from (e.g. win a race condition).

Furthermore, the baker in charge of baking can deliberately include own transactions and order the transactions for their benefit.

## Remediation
It is recommended to design a solution, which is not prone to transaction ordering attacks. In order to prevent such attack vectors consider the following measures:

- implementation of commit & reveal scheme
- requesting in entrypoints also for an expected minimal (output) value (E.g. a token exchange entrypoint could ask for a minimal expected amount of converted tokens ("slippage"). If this minimal expected amount is not realised, the token exchange is not executed.)

Note: Tezos (will) offer with the "Hangzhou" upgrade a new feature called ["time lock encryption"](https://tezos.gitlab.io/011/timelock.html).

## References
- https://tezos.gitlab.io/011/timelock.html
- https://ligolang.org/docs/tutorials/security/security#transaction-ordering
- https://tezos.stackexchange.com/questions/3522/what-happens-if-there-are-two-operations-calling-the-same-contract-in-the-same-b

## Samples/Test cases
