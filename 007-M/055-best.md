tsvetanovv

medium

# Missing deadline check when perform swap

## Summary
Missing deadline check when perform swap operations.

## Vulnerability Detail
Missing deadline checks allow pending transactions to be maliciously executed in the future. You need to add a deadline parameter to all functions which potentially perform a swap on the user's behalf.

## Impact
Without deadline parameter, as a consequence, users can have their operations executed at unexpected times, when the market conditions are unfavorable.

You need to add deadline parameter in all swap functions.

## Code Snippet
For example check [Periphery.sol](https://github.com/sherlock-audit/2023-03-sense/blob/main/sense-v1/pkg/core/src/Periphery.sol#L500-L529)
```solidity
function _balancerSwap(  
        address assetIn,
        address assetOut,
        uint256 amountIn,
        bytes32 poolId,
        uint256 minAccepted,
        address payable receiver
    ) internal returns (uint256 amountOut) {
        // approve vault to spend tokenIn
        ERC20(assetIn).safeApprove(address(balancerVault), amountIn); //@audit-ok approve by zero

        BalancerVault.SingleSwap memory request = BalancerVault.SingleSwap({
            poolId: poolId,
            kind: BalancerVault.SwapKind.GIVEN_IN,
            assetIn: IAsset(assetIn),
            assetOut: IAsset(assetOut),
            amount: amountIn,
            userData: hex""
        });

        BalancerVault.FundManagement memory funds = BalancerVault.FundManagement({
            sender: address(this),
            fromInternalBalance: false,
            recipient: receiver,
            toInternalBalance: false
        });

        amountOut = balancerVault.swap(request, funds, minAccepted, type(uint256).max);
        emit Swapped(msg.sender, poolId, assetIn, assetOut, amountIn, amountOut, msg.sig);
    }
```
## Tool used

Manual Review

## Recommendation
Introduce a `deadline` parameter in swap functions.