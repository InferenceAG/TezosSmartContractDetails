# Title
Use of BALANCE instruction

## Description
There are two important particularities to know on how the BALANCE instruction in Tezos works:

1. The balance obtained via the BALANCE instruction does not get updated during the execution of the entrypoint's own code, but keeps the value from the time when the entrypoint is called.
2. The balance of a smart contracts gets only updated when an emitted operation gets effectively processed.

## Remediation
Carefully use the BALANCE instruction and potenitally use a variable in storage or a local variable in order to track the balance of the smart contract.

## References

## Samples/Test cases
### Example regarding particularity 1.
Simple example written in SmartPy*. See directly in [SmartPy IDE](https://smartpy.io/ide?code=eJx9UU1PwzAMvfdXWOWSiGnqrkiTkLjsDvcopC5YpE4UG9D49SSl06od8Cl6H5bfC805FQWZfdF8Bi8guetC9CLwjDxiMZL3T4m1@KD2oYM6jxXCipxdTsS6YCNOoN_JiWJ2Up1GME47KBiQvrCs1jbVvQgu1K4hij_mYO3_olcfPQe0cAcnLLiBgOrlSjHCYRh29RrJpAj6jpDbipH4DSIx7ruOJugV5xy9ovTASYEYnGM_o3PXiH4cXVWoacSxf6nP3l7DNmabCo5_OUSdBGRfKJlNHuKPjcKHkD5ZTd_wuvWqW0qvyrV9e8PUVtQRk5KPbg1vLv0Nw7ZBuD@uptv1@5ufqje0tAVF7C8u9qT7). 

\* Example is written in SmartPy, but this is not limited to SmartPy. Please provide examples in other languages. We are happy to list them. 
```
import smartpy as sp

class Sender(sp.Contract):
    @sp.entry_point
    def two_step_send(self, receiver):
        sp.send(receiver, sp.tez(1))
        sp.send(receiver, sp.balance) # Here sp.balance is still 100, despite the preceding line.

if "templates" not in __name__:
    @sp.add_test(name="Test")
    def test():
        s = sp.test_scenario()
        sink = sp.test_account("sink")

        sender = Sender()
        sender.set_initial_balance(sp.tez(100))
        s += sender

        sender.two_step_send(sink.address)
```

### Example regarding particularity 2.
Please see https://github.com/InferenceAG/TezosSecurityBaselineChecking/blob/master/testcases/TC-002b/readme.md