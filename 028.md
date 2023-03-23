0x52

medium

# Multiple functions aren't payable so quotes that require protocol fees won't work correctly

## Summary

There are multiple functions that use quotes but that aren't payable. This breaks their compatibility with some quotes. As the [0x docs](https://docs.0x.org/0x-swap-api/guides/use-0x-api-liquidity-in-your-smart-contracts) state: `Certain quotes require a protocol fee, in ETH, to be attached to the swap call`.

The following flows use a quote but the external/public starting function isn't payable:

RollerPeriphery
1) redeem

Periphery
1) removeLiquidity
2) combine
3) swapPT
4) swapYT
5) issue

## Vulnerability Detail

See summary.

## Impact

Functions won't be compatible with certain quotes causing wasted gas fees or bad rates for users

## Code Snippet

https://github.com/sherlock-audit/2023-03-sense/blob/main/auto-roller/src/RollerPeriphery.sol#L104

https://github.com/sherlock-audit/2023-03-sense/blob/main/sense-v1/pkg/core/src/Periphery.sol#L325

https://github.com/sherlock-audit/2023-03-sense/blob/main/sense-v1/pkg/core/src/Periphery.sol#L409

https://github.com/sherlock-audit/2023-03-sense/blob/main/sense-v1/pkg/core/src/Periphery.sol#L433

https://github.com/sherlock-audit/2023-03-sense/blob/main/sense-v1/pkg/core/src/Periphery.sol#L240

https://github.com/sherlock-audit/2023-03-sense/blob/main/sense-v1/pkg/core/src/Periphery.sol#L263

## Tool used

Manual Review

## Recommendation

Add payable to these external/public functions