0x52

medium

# fillQuote uses transfer instead of call which can break with future updates to gas costs

## Summary

Transfer will always send ETH with a 2300 gas. This can be problematic for interacting smart contracts if gas cost change because their interaction may abruptly break.

## Vulnerability Detail

See summary.

## Impact

Changing gas costs may break integrations in the future

## Code Snippet

https://github.com/sherlock-audit/2023-03-sense/blob/main/sense-v1/pkg/core/src/Periphery.sol#L902-L932

## Tool used

Manual Review

## Recommendation

Use call instead of transfer. Reentrancy isn't a concern since the contract should only ever contain the callers funds. 