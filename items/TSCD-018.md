# Title
Converting Integers to Natural by using "abs"

## Description
Using the function "abs()" to convert a number of type Int to a number of type NAT can lead to a vulnerability in situation where the Integer to be converted can get negative.

## Remediation
It is recommended to use a dedicated function to convert numbers of type "int" to type "nat". For example:
- Michelson: ISNAT
- SmartPy: sp.is_nat() / sp.as_nat()

If there is no such function available, check before using "abs" that the number to be converted is not negative.

## References


## Samples/Test cases
Simple artificial code snippet to demonstrate the issue in SmartPy*. See directly in [SmartPy IDE](https://smartpy.io/ide?code=eJydVE1v2zAMvedXEOrFwVKhKdAdCmRY0XXYDu0pd0Ox6USoLHmSHNf_fpRsx3bhddiCHBKRfHyPX1ewN6@oH432VmQeruHpTZSVQiiMBalU7cjg5Rmhqm1lHDowWrV8tZJlZawHVwrrqxaEA1etVpkSzs1BE1fx4ff6fgX0ybGANJVa@jRNHKqifw@f8JcHU6LrMgI52BE218Intzfr9Sq6XhEIcTNtdOmfnihLWxmpPXgzOAATpam1Z2AKeg6AHH6YBs9oN4BjCBlqi@BPwgN9SyJR1iUI1YjWwfamDwZBTmT0mPOY9yuRizBpxLkonBKMKjfQMZmovYL9CSGj8oTyN1Z6tJAbKrM2xOitQnruGAFrrNFH1oOMaoLWAw7pMAdJvagPpfREEYTOYZLOSZ0FjRiThWzkLZQzUJPpSMU6OBYQiRMVyENhTQm@rRB@vuyD4eVhvyEAClMo8phdwLlWGq04SCV9C80JqUbaNAPDTOhAsasaSO08hQYFF9J8NSH5LF0kk50wew1oxNcCi6ORCy_4OBrXMHSXCGk8xmHlM8UVJyGyaJOl@C@7oSmXEI1NND50dY7Dp0wmVMJeZiZGDT24RdiB1nqEnVB6NDlSc8dNi_XtKu6k0ZdeuJSmnk0i_4UbmTqA5GOKGyjROXFEgmMTEmECaeWdPChkS0LGna24LCCZk@NnoWoMBe6Xd0vLO07@Zdff0drBEsw0FSqH73AqXgipGulPCXse9vayJhZ_1dLS4PXz1w@lDEvNJrpIA_NITREeaQmCfqnpUGlRYpreX5Zd5HlKHnShyBCKNrt3PWK4ANFpojnbkvf8OI7pXYZaWGm6pobQdHha8OKnbcK@9ReuE8QWsD7tKOkkP58fpa4xd@sF_H5nQsi7Bo0NvRuu8Wwa_pAltJ_bWifUUJnvvtPRwfXfgm4__y@3beD2GxYCLBE-). 
* Example is written in SmartPy, but this is not limited to SmartPy. Please provide examples in other languages. We are happy to list them.
```
# TokenContract - Example for illustrative purposes only.

import smartpy as sp

class TokenContract(sp.Contract):
    def __init__(self):
        self.init(numTokens = sp.nat(20))

    # destroyToken
    # Entrypoint to destroy "amount" of tokens. However, entrypoint ensure that at minimum always 10 tokens are minted.
    @sp.entry_point
    def destroyToken(self, amount):
        # The contact writer does not expect that a "wrong" amount of tokens to be destroyed is submitted and 
        # since the writter is also using "abs" to convert from type INT to NAT, this leads to a vulnerability where now tokens can be minted instead of destroyed.

        # Missing check whether "self.data.numTokens - amount" is negative. 
        # sp.verify(self.data.numTokens >= amount)
        newTokenAmount = sp.local("NewTokenAmount", abs(self.data.numTokens - amount))
        
        # Code example for type conversion using "as_nat"
        # newTokenAmount = sp.local("NewTokenAmount", sp.as_nat((self.data.numTokens - amount), message = "conversion not possible"))
        
        
        sp.if (newTokenAmount.value >= sp.nat(10)):
            self.data.numTokens = newTokenAmount.value
        sp.else:
            sp.failwith("Minimum amount of required minted tokens is 10")
        
if "templates" not in __name__:
    @sp.add_test(name = "TokenContract")
    def test():
        c1 = TokenContract()
        scenario = sp.test_scenario()
        scenario.h1("Destroy tokens")
        scenario += c1
        c1.destroyToken(sp.nat(5))
        scenario.verify(c1.data.numTokens == sp.nat(15))

        
        c1.destroyToken(sp.nat(10)).run(valid=False)
        c1.destroyToken(sp.nat(26))
        scenario.verify(c1.data.numTokens == sp.nat(11))

```
