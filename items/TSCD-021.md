# Title
SmartPy - inadvertent meta-programmation with control statements

## Description  
Control statements in SmartPy may inadvertent lead to a wrong meta-programmation. See code example below


## Remediation
It is recommended to carefully review SmartPy code containing control statements and to implement appropriate test cases to cover expected working of control statements. 

## References


## Samples/Test cases
Code example for an sp.if control statement written in SmartPy. Can also be viewed in [SmartPy IDE](https://smartpy.io/ide?code=eJx9Us1qwzAMvvsphHdJWBeWHguFwdgTbOxqTOJ0Bsc2llrWt5@cdHG6lvkofT_6JD_AO4Vk4FO7o4EnePvWY3QGhpDAOndESprsyUA8phjQIATvzo0QdowhEeCoE8UzaASMQnROI86Sk2KFsXkNnkU6qncC@PVmAKWst6RUhcYNGzhl6KWdX642GVFhVurn4fYzbgOjPthOBZ9LH4mZYmK@sJdhq7OKwXpazNDQ5LMymBV6TZolnotvbOwwu@desxgV5g27XbONw_@w2@uEk8d1wgLnDQ8gyfA1NBmU4AOB9bw6r0ej1G7JrPteMYKq3GANWdYv62ULE2K1gq5l6OpQ7bYu03XG62QDI1g_M9VvqbpFNV9tJVe_SN4Retyz4T39rm3yfQpFXFaZU3VhjJbj2@AV6XRgoMQSbvNn_voH29PWxA--).
```
# Store Value - Example for illustrative purposes only.

import smartpy as sp

class StoreValue(sp.Contract):
    def __init__(self, value):
        self.init(storedValue = value, magic_one = True)

    @sp.entry_point
    def set(self):
        magic_data = 0
        sp.if self.data.magic_one:
            magic_data = 1
        sp.else:
            magic_data = 2
        self.data.storedValue = magic_data

if "templates" not in __name__:
    @sp.add_test(name = "StoreValue")
    def test():
        c1 = StoreValue(12)
        scenario = sp.test_scenario()
        scenario.h1("Store Value")
        scenario += c1
        scenario = c1.set()
        
    sp.add_compilation_target("storeValue", StoreValue(12))
```

Solution 1 with direct modification. See directly in [SmartPy IDE](https://smartpy.io/ide?code=eJyNUk1LxDAQvedXDPHS4hroHhcEQfwFitcQ0nQNpEnIzC7uv3fSrm3VPTjHyfuY98gdvFIqDt5NODl4gJdPM@bgYEgFfAgnpGLInx3kU8kJHUKK4aKE8GNOhQBHUyhfwCBgFsIGgzhLTooNZvWcIotYag8CeHo3gNY@etK6QReGHZwr9Ppcp25VRTRYlfr5uMcZt4PRHL3VKdbVW2GmmJhP7OXY6qJz8pEWM3Q0@WwNsvLDbNMbMmpRXCHLHRPg5x3dVsgF_Cdtz60NIMlxw4YcSoiJwEeuI5rRaX1Ycpi@14ygpj4wVa6VynZJNiE2sWzH0E353b5dL7UumuITI1i_MvX3qvmLUh9dIzc_Q94Qun9kw1v6tlO185UirlXVVDaN2XN8n6ImU44MlLiG2_26v_0CDlvLzA--).
```
# Store Value - Example for illustrative purposes only.

import smartpy as sp

class StoreValue(sp.Contract):
    def __init__(self, value):
        self.init(storedValue = value, magic_one = True)

    @sp.entry_point
    def set(self):
        sp.if self.data.magic_one:
            self.data.storedValue = 1
        sp.else:
            self.data.storedValue = 2

if "templates" not in __name__:
    @sp.add_test(name = "StoreValue")
    def test():
        c1 = StoreValue(12)
        scenario = sp.test_scenario()
        scenario.h1("Store Value")
        scenario += c1
        scenario = c1.set()
        
    sp.add_compilation_target("storeValue", StoreValue(12))
```

Solution 2 using locals. See directly in [SmartPy IDE](https://smartpy.io/ide?code=eJyNUk1rAyEQve@vGOzFpamwOQYChdJf0NKrDK6bCq6KTkLz76u7m41Jc6jH8X2M7_kEH@Sjhi@0Rw0v8P6DY7AaBh_BWHtMFJHMSUM4xuCTTuCdPYumMWPwkSCNGCmcAROk0DTKYkqz5KTIUxBv3mURRe2ugXx6PYCUxhmSkidthw2cCnS5LqdMRUHwVJT6ebn9jNvAiAejpHdl9Bkzs5mYr9lLZ6uzDN44Ws2SpsmnNriRzTzrFVrOqjnbzGv0SCiqeXsVCcIMFWhd6@pz5yVOi2NXi2ib_kHZ3qZzv1Z5xj0rlzQAI50LRdKJgfMExuX0HY5ayt0aG_a9zAji5SJLsWuDrF2DnBBViqrL0KrrbluFo7TDaPwcb2HKy4j_RYnvjrPqI7IHQs_7bPhIX3WiVLz8guU1yo_B5Gcb7yRhPGTA3O6l3Nu92@YX8m7viA--).
```
# Store Value - Example for illustrative purposes only.

import smartpy as sp

class StoreValue(sp.Contract):
    def __init__(self, value):
        self.init(storedValue = value, magic_one = True)

    @sp.entry_point
    def set(self):
        storedValue = sp.local("storedValue", self.data.storedValue)
        sp.if self.data.magic_one:
            storedValue.value = 1
        sp.else:
            storedValue.value = 2
        self.data.storedValue = storedValue.value

if "templates" not in __name__:
    @sp.add_test(name = "StoreValue")
    def test():
        c1 = StoreValue(12)
        scenario = sp.test_scenario()
        scenario.h1("Store Value")
        scenario += c1
        scenario = c1.set()

    sp.add_compilation_target("storeValue", StoreValue(12))

```
