tsvetanovv

medium

# Solmate's SafeTransferLib doesn't check whether the ERC20 contract exists

## Summary
The `safeTransfer` and `safeTransferFrom` don't check the existence of code at the token address. 

## Vulnerability Detail
This may lead to miscalculation of funds and may lead to loss of funds, because if `safeTransfer()` and `safeTransferFrom()` are called on a token address that doesn't have a contract in it, it will always return success, bypassing the return value check.

## Impact
Due to this protocol will think that funds have been transferred successfully, and records will be accordingly calculated, but in reality, funds were never transferred. So this will lead to miscalculation and possibly loss of funds.

## Code Snippet

```solidity
https://github.com/sherlock-audit/2023-03-sense/blob/main/sense-v1/pkg/core/src/Divider.sol#L7
import { SafeTransferLib } from "solmate/utils/SafeTransferLib.sol";
```

## Tool used

Manual Review

## Recommendation

Use Openzeppelin's safeERC20 or implement a code existence check.