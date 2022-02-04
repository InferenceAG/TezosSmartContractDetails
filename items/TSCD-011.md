# Title
Unexpected tez

## Description
Smart contract developers may assume only to receive tez on some specific entrypoints, but do not reject received tez on other entrypoints. This may lead to a vulnerability where tez is locked in the smart contract in case the smart contract does not have any possibility to send tez out. The contract may also become unusable if the smart contract code strictly assumes a specific tez balance. 

## Remediation
It is recommended to avoid strict equality checks for the tez balance in a contract. Furthermore, it is recommended to carefully design the smart contract with regards to accepting tez on entrypoints and the possibility to send out mistakenly received tez.

## References

## Samples/Test cases
### Example in Ligo

The following contract only accepts 2 tez with each spending. As soon as 10 tez of spendings are reached the contract is emptied and the tez in the contract is sent to the address defined in the storage. If somebody now calls the entrypoint "getBalance", but also sends some tez with, the contract gets unusable and all tez are locked, since the check "Tezos.balance = 10tez" can no longer be fulfilled. (Note: This particular demo smart contract could be recovered as long as the balance is less than 8 tez. This would be done by also using the "getBalance" entrypoint to stock up the balance to exactly 2, 4, 6, or 8 tez. If the balance is already higher than 8 tez then all tez in the contract are locked.)
```
// Disclaimer! 
// This is a demo contract and contains vulnerabilities. Thus, do not use this code for any real-world cases!
// DO NOT USE!

type storage is address;

type parameter is
| Spend of unit
| GetBalance of contract(tez);

function getBalance(const store : storage; const param: contract(tez)): (list(operation) * storage) is
block {
    const opGetBalance : operation = Tezos.transaction(Tezos.balance, 0tez, param);
    const txs : list(operation) = list[opGetBalance];   
} with (txs, store)

function spend(const store : storage; const _param: unit): (list(operation) * storage) is
block {
    var txs : list(operation) := nil;
    if Tezos.amount =/= 2tez then failwith("contract only accepts 2 tez");
    else block {
        if Tezos.balance = 10tez then block {
            const receiver : contract(unit) = case (Tezos.get_contract_opt(store) : option(contract(unit))) of
            | None -> failwith("none")
            | Some(x) -> x
            end;
            const opPayout : operation = Tezos.transaction(unit, 10tez, receiver);
            txs := list[opPayout];   
        }
        else skip;
    };
} with (txs, store)

function main (const action : parameter; const store : storage): (list(operation) * storage) is
block {
    skip
} with case action of
| Spend(param) -> spend(store, param)        
| GetBalance(param) -> getBalance(store, param)
end;
```

### Example in SmartPy
View in [SmartPy IDE](https://smartpy.io/ide?code=eJzVVkuPmzAQvudXjNhDQU1RslIvK0XqQ31c2ksfV@TAsFgF28Im2fTXdww2AUI23e1KbSMhRfbMfDOfvxn7Cr4JvFOYGszA4E94Ae_uWKVKhFzWwMuy0aZmhu8QVFMrqVGDFOUhXix4pWRtQFesNuoATINWi0VaMq3hi0KRYR1qFb@VgiKkJrpZAP0yzCFJuOAmSUKNZb6EGlMkgPp1ltWotTO0P7sfW9vQ28DmxHzRmr8iKCSkQ6IkF6bHukXzhpVMpOjQUlaWW5b@GMBcwUesEfYIupBNmUHBdjjYpdCEx_ODrYdVshEGNhu7TJSFq2gJFSXCbpGyCz7LlkhCkXsitaDIQXSsSMUaTWIOCkOfydKufvU8WYyvnxqKEY3caFPovON021XUOlaNT6KvrHW7lxVtz6clZMi2inkOszVeRwO7sa3LZWC8Xk2t@8odaJwxw2J_kG0ZLsyoZCw1nsDmjJd7boowSB1jrSCBpSkqo@Ha0k@MOyV@57j_PSHO6q6vbkj1BcnRx5rSTIMqSud@Rz3V6nFhfExeQgn1LtXWW3VK6sQzLqYlfFBM70J9nENgkHqeGdQBCGmAC6JGsAqT5KbPmGVZQhYmtBtW6a7JnbptAe32INVdS74nnnzcaQxPmWIMDPzgGHta7K7Xe78UBau5BKc6bRK_FJ5axcU67NIFRp9vpWDO8joMPKyeMYDnm3HSsxbj9OdRXnZxgjNkxF2PRnHdiLBryM2xG_9pn8HonCjAtTwdQYGwXtlWHRjvCxQTRRR0reAd_aMOn9j3ZLrZPJHM6Vxa_SkFHpBuif0ZuHMIw2tIxenJrF_CvOKXEBx7NYhiSVETLSsMoy7dh6b3ePrWq2jO3Uq5bS0ubtvh0Q1ifIyw7S22YyXPNu8Zzf5zcB_QwNZT8vcIv5gc2HuqfQ34iVOPaBm8O@xoWkN7wVAbcHp_ceNeI@IZBaTnl9Sab0u8XO_0xnyS4vug03PrLsV1dIkXN@46TjxBuuS3he3tSmY8548Szf87Dff0vm5bxo1FPxUrSaoYj8YtpqzRncV4QnZB0L6N7UEzLp5uYq6fbGQ@CPEX4YTy9g--)
```
# Unexpected tez - Example for illustrative purposes only.

import smartpy as sp

class Spender(sp.Contract):
    def __init__(self, receiverAddress):
        self.init(receiver = receiverAddress)

    @sp.entry_point
    def getBalance(self, callback):
        # Here we should have
        # sp.verify(sp.amount == sp.tez(0), message = "No tez allowed here")
        sp.set_type(callback, sp.TContract(sp.TMutez))
        sp.transfer(sp.balance, sp.mutez(0), callback)
    
    @sp.entry_point
    def spend(self):
        sp.if (sp.amount == sp.tez(2)):
            sp.if (sp.balance == sp.tez(10)):
                sp.send(self.data.receiver, sp.balance)
        sp.else:
            sp.failwith("contract only accepts 2 tez")

class Viewer(sp.Contract):
    def __init__(self):
        self.init(balance = sp.mutez(0))

    @sp.entry_point
    def default(self):
        pass

    @sp.entry_point
    def setBalance(self, setBalance):
        sp.set_type_expr(setBalance, sp.TMutez)
        self.data.balance = setBalance

if "templates" not in __name__:
    @sp.add_test(name = "Spender")
    def test():
        viewerContract = Viewer()
        spendContract = Spender(viewerContract.address)
        scenario = sp.test_scenario()
        scenario.h1("Spend and transfer")
        scenario.h2("Contracts")
        scenario += spendContract
        scenario += viewerContract
        scenario.h2("5 spend")
        spendContract.spend().run(amount=sp.tez(2))
        spendContract.spend().run(amount=sp.tez(2))
        spendContract.spend().run(amount=sp.tez(2))
        spendContract.spend().run(amount=sp.tez(2))

        # viewerContract received the 10 tez
        # when spendContract has exactly 10 tez
        scenario.verify(viewerContract.balance == sp.tez(0))
        spendContract.spend().run(amount=sp.tez(2))
        scenario.show(viewerContract.balance)
        spendContract.getBalance(sp.contract(sp.TMutez, viewerContract.address, "setBalance").open_some()).run()
        scenario.show(viewerContract.balance)

        scenario.verify(viewerContract.balance == sp.tez(10))

        scenario.h2("Spending not accepted")
        spendContract.spend().run(amount=sp.tez(0), valid=False)

        scenario.h2("Get balance")
        spendContract.getBalance(sp.contract(sp.TMutez, viewerContract.address, "setBalance").open_some()).run()
        
        scenario.h2("Get balance with tez transferred")
        # Here we send 1 mutez while it shouldn't be possible
        spendContract.getBalance(
            sp.contract(sp.TMutez, viewerContract.address, "setBalance").open_some()
        ).run(amount=sp.mutez(1))
        
        scenario.h2("5 spend with balance slightly modified")
        spendContract.spend().run(amount=sp.tez(2))
        spendContract.spend().run(amount=sp.tez(2))
        spendContract.spend().run(amount=sp.tez(2))
        spendContract.spend().run(amount=sp.tez(2))

        # viewerContract will not receive the 10 more tez
        # because the spendContract will never contains exactly 10 tez
        scenario.verify(viewerContract.balance == sp.tez(10))
        spendContract.spend().run(amount=sp.tez(2))
        scenario.verify(viewerContract.balance == sp.tez(10))
```