# Title
Transaction delay

## Description
Transactions foreseen to be executed at a specific time may be e.g. delayed due to errors or a congested blockchain. Thus, the transaction gets executed at a later time.

Note: The current protocol Granada includes a native deadline in the protocol on operation level, which is 120 blocks.

## Remediation
It is recommended to add a deadline parameter to an entrypoint, where the functionality / design of the smart contract requires one. This deadline is then checked by the entrypoint whether execution is not beyond the deadline.

## References


## Samples/Test cases
