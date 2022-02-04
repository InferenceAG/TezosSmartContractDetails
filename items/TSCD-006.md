# Title
Authorisation checks with source

## Description
SOURCE returns the account that initiated the current transaction. Using this variable for authorization checks may lead to a vulnerability, where this authorized account calls a malicious contract (“MitM”/relayer/proxy attack). The malicious contract could then act on behalf of the authorized account.  

## Remediation
It is recommended not to use SOURCE for authorization, but SENDER, which returns the address of the immediate caller.

## References


## Samples/Test cases
