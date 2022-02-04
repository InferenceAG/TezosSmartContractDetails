# Title 
Mutez overflow

## Description
Mutez type in Michelson is a 64bit unsigned integer. This could lead to a runtime error due to mutez overflows and thus ultimately to an unusable smart contract. Mutez underflows are also possible, but do generally not pose a problem, since they are generally expected to fail, since non-negative mutez values are not expected.

## Remediation 
It is recommended to convert mutez types to nat types for calculations, especially in designs where large numbers are expected or are very likely to occur. For instance, designs, where tez values are multiplied with a factor to increase calculation precision, an overflow would occur, if 1000 tez * 10^12n is calculated.

### Note 
A merge request is open, which would resolve this “Mutez overflow” weakness: https://gitlab.com/tezos/tezos/-/merge_requests/3020

## References
- https://tezos.gitlab.io/michelson-reference/#type-mutez
- https://tezos.gitlab.io/michelson-reference/#instr-MUL

## Samples/Test cases 
### Overflow
Mutez overflow example in Michelson, which tries to multiple 1000 tez with a precision factor of 10^12:
```
parameter unit ; storage mutez ; code { CDR ; DROP; PUSH mutez 1000000000; PUSH nat 1000000000000; MUL; NIL operation ; PAIR }
```

This code results in the error message:
```
Overflowing multiplication of 1000 tez and 1000000000000
Overflowing addition of 8589934592000 tez and 8589934592000 tez
```

### Underflow
Sample Michelson script to demonstrate a mutez underflow.
```
parameter unit ; storage mutez ; code { CDR ; DROP; PUSH mutez 4; PUSH mutez 2; SUB; NIL operation ; PAIR }
```

This Michelson code results in the error message: 
```
Underflowing subtraction of 0.000002 tez and 0.000004 tez
```