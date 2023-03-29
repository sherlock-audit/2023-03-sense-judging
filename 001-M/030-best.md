0x52

medium

# safeApprove breaks logic and functions in many locations

## Summary

safeApprove will revert if there is any leftover allowance. This causes broken logic in many spots. 

Periphery#sponsorSeries - full allowance may not be used causing that stake ERC20 to be broken afterwards

Periphery#onBoardAdapter - will fail if the same target is used across multiple different adapters

Periphery/RollerPeriphery#_fillQuote - always approves max which will cause revert on subsequent calls for same token to same spender

## Vulnerability Detail

See summary.

## Impact

Users will be unable to sponsor series or create adapters with those tokens

## Code Snippet

https://github.com/sherlock-audit/2023-03-sense/blob/main/sense-v1/pkg/core/src/Periphery.sol#L128

https://github.com/sherlock-audit/2023-03-sense/blob/main/sense-v1/pkg/core/src/Periphery.sol#L491

https://github.com/sherlock-audit/2023-03-sense/blob/main/sense-v1/pkg/core/src/Periphery.sol#L908

## Tool used

Manual Review

## Recommendation

Check if current allowance is enough and only increase if needed