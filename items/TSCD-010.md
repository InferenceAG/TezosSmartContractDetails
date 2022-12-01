# Title
Unused variables and code 

## Description
Unused variables and code may indicate wrong programming logic. In addition to increasing storage and transaction fees, dead or unreachable code unnecessarily increases the attack surface, this is specially the case, if the code is an entrypoint.

Unused variables may are also an indicator that there is an error in the code. See our example below in section "samples/test cases".

## Remediation
It is recommended to remove all unused variables and code elements from the code.

## References


## Samples/Test cases
The following smart contract example implements a list which never should grow larger than "10" elements. However, due to a missing dot in "runnervalue" (correct would be "runner.value"), the list actually can grow larget than 10 elements. Actually the list can now grow indefinitely and potentially lead to gas exhaustion issues when loading the smart contract for execution.

Can also be viewed in [SmartPy IDE](https://smartpy.io/ide?code=eJyVVU1v2zAMvftXsO7FRjMj2feKBRiwj1Oxw9CdhkFQbSYRIEuCRKfNvx9lO66drAisk0w@Uu_RT_Y1_DZNwAr20iv5oBFewfcnWTvebawHpXUTyEtSewTXeGcDBrBGH4pE1c56glBLT@4AMkBwSVJqGQL8UE9Y3aHZ0u5OBcqCK75aw41Kym8T4FXhBoRQRpEQWUC9WUCfiSsGijZJB4ex_P4XltZX2WbaGdYQk8dD7n9KyvP8vE82hOL6bxMdezyXPu_azRdGICs4CGeVoUGDrKqe_l7qBscaXBGwF9DmFnBkmAwg3xiDvidgS6mztAulLdpIypYjPQYfR4Q7fB_rCloRev3n7wJoPQwkmQ6kkiSLkxkUrgm7jucY7opoA9RYgzIvV99OxstVatNLK9qW8PmoZrXMp@CRrA7bMYlHjojEdQ1XV1fwzRLs2GwPiCZ6dGuJeEeW2dECHneq3IFGWYUYkwaaE4f3A27PSguYHDFK8YwnCm5g1Zrh8ii5cirI456NlSQ8lJSQL5ckDCkYVsJDFcLIGoXoxjKYjY0lGEZZzHLLND6kOQzOa5Njv5VoWKPtzBGz4hga2fqE6_FWctHpnc3PO9@sX6q_1L9o70lvgfwinTH89Tz4m3nwt_Pg7@bB38@Df5gH_zgP_mkefLWciZ_5XlfxxSZJb_XS1k7xxVDWCJJ@i5Sl2P2J@Mt25s38H8twFcg-).

```
# Unused variable - Example for illustrative purposes only.

import smartpy as sp

class FixedLengthList(sp.Contract):
    def __init__(self, ):
        self.init_type(sp.TRecord(fixedLengthList = sp.TList(sp.TNat)))
        self.init(
            fixedLengthList = sp.list()
        )
        
    @sp.entry_point
    def add(self, value):
        sp.set_type(value, sp.TNat)

        runner = sp.local("runner", sp.nat(0))
        newList = sp.local("newList", sp.list(l=[], t=sp.TNat))

        self.data.fixedLengthList.push(value)

        sp.for elem in self.data.fixedLengthList:
            sp.if runner.value < sp.nat(10):
                newList.value.push(elem)
            
            # !!! Dot has been forgotten to set, which leads to an unused variable "runnervalue".
            runnervalue = runner.value + 1    

        self.data.fixedLengthList = newList.value.rev()


if "templates" not in __name__:
    
    @sp.add_test(name = "test") 
    def test():
        scenario = sp.test_scenario()
        fixedLengthListContract = FixedLengthList()
        scenario += fixedLengthListContract
        fixedLengthListContract.add(sp.nat(1))
        fixedLengthListContract.add(sp.nat(2))
        fixedLengthListContract.add(sp.nat(3))
        fixedLengthListContract.add(sp.nat(4))
        fixedLengthListContract.add(sp.nat(5))
        fixedLengthListContract.add(sp.nat(6))
        fixedLengthListContract.add(sp.nat(7))
        fixedLengthListContract.add(sp.nat(8))
        fixedLengthListContract.add(sp.nat(9))
        fixedLengthListContract.add(sp.nat(10))
        fixedLengthListContract.add(sp.nat(11))
        fixedLengthListContract.add(sp.nat(12))


sp.add_compilation_target("example", FixedLengthList())
```
