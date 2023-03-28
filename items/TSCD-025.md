# Title
SmartPy - using sp.local together with map / big_maps

## Description  
Using sp.local to handle values from maps / big_maps may lead to a vulnerability in case the sp.local variable is not stored back to the map. / big_map.

## Remediation
It is recommended to carefully review SmartPy code containing sp.local together with map or big_maps and to implement appropriate test cases to cover expected working of code.

## References


## Samples/Test cases
Code example. Can also be viewed in [SmartPy IDE](https://smartpy.io/ide?code=eJylkjFvwjAQhff8ipNZEpVGwIiEVAl17FSJpaosN7mAVce2bIOaf9@zCSGg0KWZnPPzd8_3PIP3YBzCTqgjwjO8_ojWKoTGOJBKHX1wIsgTgj06azx6MFp1ZZbJ1hoXwLfCBduB8OBtllVKeH9GJmLubbk1miBVKNYZ0FdjA5xLLQPnuUfV9PX4xd8ybuU@IurE2BDjS@55K2xeFFkSv1ANCdtxa6QOA9hjuGee0s02sLjtUosgylGXj8UniZL43GIG24PQe4S@dw@Kg_nGDtiCQTDAVuxPQ1Vi3HsiwK63ReeUqYTK2aXI5g8dFlOEFYXRAAtIwYmAnoE2AaSmKWvRIufrwaGoa06KkMcNOsquSbFi8JwUI7fVkqSjTK8ufIVaOGnO94jn@KU0oSoPy5yNnhubAD1tqN2DchnTncCe0Mmmy0kwGSpF_6hR2cdz3R8WM3gztWwk1n3w0qe5nulxvP27WP_PUFr00VSmtZIylEbzINyersv8NaH5TQjFL_nuIHw-).

```
# Store Value - Example for illustrative purposes only.

import smartpy as sp

class StoreValue(sp.Contract):
    def __init__(self):
        self.init(storedValue=sp.big_map())

    @sp.entry_point
    def set(self):
        value = 0
        self.data.storedValue[0] = value

    # Change big_map value for key "0" to "2"
    @sp.entry_point
    def change(self):
        keyValue = sp.local("keyValue", self.data.storedValue[0])
        keyValue = 2

if "templates" not in __name__:
    @sp.add_test(name = "StoreValue")
    def test():
        c1 = StoreValue()
        scenario = sp.test_scenario()
        scenario.h1("Store Value")
        scenario += c1
        scenario += c1.set()
        scenario.verify(c1.data.storedValue[0] == 0)
        scenario += c1.change()
        
        # Modified value is not stored in big_map:
        scenario.verify(c1.data.storedValue[0] == 0)
        
    sp.add_compilation_target("storeValue", StoreValue())
```

Solution. See directly in [SmartPy IDE](https://smartpy.io/ide?code=eJydk0Frg0AQhe_@imFzUZpKkmNAKIQeeyrkUsqy1TFZuu4uu5tQ_31njVEbTAv1pLPP7834xgW8BuMQ9kKdEB7h@Us0ViHUxoFU6uSDE0GeEezJWePRg9GqzZNENta4AL4RLtgWhAdvk6RUwvsLsiOm3uY7owlShmybAF0V1sC51DJwnnpUdV@PV3zM41HqI6LqGAUxPuSBN8KmWZZ04ieqIWFbbo3UYQB7DLfMczdZAaufLpUIIp@4vK3eSdSJLxYL2B2FPiD03j0ofphPbIGtGAQDbMN@bajsGLc9EWDft0XvKVMKlbJrkS3vdpjNETZ_DlZc1RRbDSwgRSwCegbaBJCa8tCiQc63wyyiqjgpQhoPyISNmbJsmK5TTOYq1ySdpD_260vUwklzmTi@x6@lGVV@XKdssphsBvRQkN2dch73YAZ7RifrNiXBbPy0JPeM8j7I8Xy4WcCLqWQtsepXRNK_0JHjp@23Z_uPZjY3Zn0spWmspPyk0TwId6BRmR_TWf4IIPsGITMssA--).
```
# Store Value - Example for illustrative purposes only.

import smartpy as sp

class StoreValue(sp.Contract):
    def __init__(self):
        self.init(storedValue=sp.big_map())

    @sp.entry_point
    def set(self):
        value = 0
        self.data.storedValue[0] = value

    # Change big_map value for key "0" to "2"
    @sp.entry_point
    def change(self):
        keyValue = sp.local("keyValue", self.data.storedValue[0])
        keyValue = 2
        self.data.storedValue[0]=keyValue

if "templates" not in __name__:
    @sp.add_test(name = "StoreValue")
    def test():
        c1 = StoreValue()
        scenario = sp.test_scenario()
        scenario.h1("Store Value")
        scenario += c1
        scenario += c1.set()
        scenario.verify(c1.data.storedValue[0] == 0)
        scenario += c1.change()
        
        # Modified value is stored in big_map:
        scenario.verify(c1.data.storedValue[0] == 2)
        
    sp.add_compilation_target("storeValue", StoreValue())
```
