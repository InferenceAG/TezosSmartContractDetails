# Title
Immediate use of call operation results

## Description
Operations crafted by a smart contract are not immediately executed. All operations are added to a list of operations, which then will be executed in DFS style ordering after the crafting function/entrypoint finishes its execution. This may lead to a vulnerability where a wrong state is processed, since smart contract developers assume that results of such operations are immediately available.

## Remediation
It is recommended to carefully analyse smart contract code whether the code is not immediately relying on its own crafted operations. Furthermore, it is highly advised to define appropriate test cases checking whether functions appropriately manipulate the contract’s state.

## References


## Samples/Test cases

