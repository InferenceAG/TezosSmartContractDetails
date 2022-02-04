# Title
Imprecise calculations

## Description
### Division
Divisions of integers, natural numbers and tez are always Euclidian divisions in Tezos resulting in two values - a quotient and a remainder. The quotient is rounded down. 

Bit shifted natural numbers to the right are also rounded down.

Calculation with rounded down numbers can lead to a weakness or vulnerability in situations where the rounding is not appropriately taken into account.

## Remediation
It is recommended to avoid Euclidian divisions and bit shifts to the right, if possible. 

Where Euclidian divisions or bit shifts to the right can not be avoided, calculations have to be treated with special care in order to prevent wrong/unfavorable calculations. Depending on the situation consider as measures:

- First doing all multiplications and doing the divisions and bit shifts to the right at last
- Introduction of a precision factor to have more precise values
- Requiring a minimum amount of tez / tokens

## References


## Samples/Test cases
Example for wrong / disadvantageous calculation and vulnerabilities
- Calculating and compounding of a yearly interest rate of 0.9%: A user having 100 tokens would never earn any interests, since 100n * 9n/1000n = 0tokens. Thus, based on the specification / definition this may be inappropriate, since accumulation of interest (compounding) would not be done, if this rate is applied all the time with an imprecise value to store the token.
- For exchanging a token of type "A" to a token of type "B", a DEX expects as a parameter the amount "b" of token "B" a user wants to buy. The DEX calculates the amount "a" of the required tokens "A" by calculating a = b * total_pool_A / total_pool_B and deducts "a" tokens from the user's token A account. Consider now an exchange rate of 1:2 (token_pool_A = 100 and token_pool_B =200). A user who want to buy 1 token of type B has to pay 0 tokens of type A due to 1*100/200.  

Example for avoiding divisions:
- A user has “a” tokens of a token where in total “A” tokens exist in the pool, but the user also has "b" tokens of "B" tokens. Of which token does the user have a bigger share? Instead of comparing “a/A” with “b/B”, you can prevent a division by comparing a * B with b * A. (Note: only valid in case "A" and "B" are both positive or negative numbers).
