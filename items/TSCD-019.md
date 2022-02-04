# Title
Change of critical addresses

## Description
Directly changing a critical address in a smart contract using a one step process, may lead to an inaccessible contract, if an incorrect address is provided.

## Remediation
It is recommended to implement a two-step process to change critical addresses in a smart contract. The first transaction registers the new address, whereas the second transaction from the newly registered address confirms and applies the change.

## References


## Samples/Test cases
Solution example in SmartPy*. See directly in [SmartPy IDE](https://smartpy.io/ide?code=eJy1VNGO2yAQfPdXbHmy1dQfUClST5X6AW3eqgoRWDeoGBDgpPn77mIn595Fl57UyxNhZ2eGYbEQotkdbAYdfElKF6C18mCdmzJtFBs8hAHKAUGUUwCtktpbj4lgZrQeoioFkxd90@wCTNGoghVey5u61FNK6MvSMRIzLQ3pBKqmeZtsoK9oj6c1MuEYjjNlcAaCR5ZizxmJ2JYzjMGgg5jwSCqLMVvtB_I5pDA2e7T@J3FFpzQa2J9BaR0mhpeDKmACkBs4hfSrbwSl0tgxhlQgjyqVSHDSi02jncoZHljh8xJZm2N_WXcfG6CfwQGkJBNFyjajGzazqbzU@cfbPUPauQTbC@Y5RJZzRNbZfUUdknls4b1vWD3sHoxJmHPXdU2l@ESbFEg6yxisL1dnlH09wMXZ2lTsj5jscK61ni5T9bNWzxOiaMFSGb3B1G1AUGiVSzxxve4kvVbd8TTf8pvaunIoeFeDm@GEZtIP1YFy_AB4EpRz4YTmpXPNnuvRGjvQ@8CRxqtgFpWBBlhKr0aUcj6JclbjfGcEKnKZwFbUwqK0D_tbENpeANcYKVbJoJY1qEfsbr3Py8Neujnr2rRKV1Pzs4nONFTfqzEW4rn60a2yWHnMGr1KNrTr8vst6EeB_jpydI4LX9eniS@bL4GHn7W6Vc96JP4y8qSRKFfDdS8V_B1RF_oEDMq6iej@czD6TjK6JvO6aKiyAZpNa@jPF@UybugcGmP9PG9vjvur8ntD9lp7mf_fnh8v_gArHSAz).
* Example is written in SmartPy, but this is not limited to SmartPy. Please provide examples in other languages. We are happy to list them.
```
"""
This contract is an illustration of the "two carabiners admin pattern".

To update the admin, the current admin must add another admin
Then the new admin must remove the old one.

This security model prevents administrators from
being replaced by accounts that do not work.
"""

import smartpy as sp

class AdminContract(sp.Contract):
    def __init__(self, admins):
        self.init(admins = admins)
        self.init_type(sp.TRecord(admins = sp.TSet(sp.TAddress)))

    @sp.entry_point
    def addAdmin(self, a):
        sp.verify(self.data.admins.contains(sp.sender), "notAdmin")
        self.data.admins.add(a)

    @sp.entry_point
    def removeAdmin(self, a):
        sp.verify(self.data.admins.contains(sp.sender), "notAdmin")
        sp.verify(a != sp.sender, "self-removal is not allowed")
        self.data.admins.remove(a)


if "templates" not in __name__:
    alice = sp.test_account("alice")
    bob = sp.test_account("bob")
    
    @sp.add_test(name = "Two carabiners admin contract")
    def test():
        c = AdminContract(sp.set([alice.address]))
        s = sp.test_scenario()
        s += c
        c.addAdmin(bob.address).run(sender = alice)
        c.removeAdmin(alice.address).run(sender = bob)

    @sp.add_test(name = "Two carabiners expected failures")
    def test():
        c = AdminContract(sp.set([alice.address]))
        sc = sp.test_scenario()
        sc+= c

        c.addAdmin(bob.address).run(sender = bob, valid = False, exception = "notAdmin")
        c.removeAdmin(alice.address).run(sender = bob, valid = False, exception = "notAdmin")
        c.removeAdmin(alice.address).run(sender = alice, valid = False, exception = "self-removal is not allowed")
        
   
```