# Title
Wrong operation call ordering

## Description
An incorrect order of operations crafted by a smart contract may lead to undesirable effects and wrong states in the smart contracts, which may lead to a vulnerability such as a reentrancy attack or token drainage attack.

## Remediation
It is recommended to carefully analyse smart contract code to determine whether the order of crafted operations are in the correct order. Furthermore, it is highly advised to define appropriate test cases checking whether functions appropriately manipulate the contract’s state. 

## References


## Samples/Test cases
