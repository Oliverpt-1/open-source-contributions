# open-source-contributions

# Contribution to GoodDollar SDK

**Commit:** [`7f1d8c8`](https://github.com/GoodDollar/GoodSDKs/commit/7f1d8c8f9698a84ef1a000f38c2d7818f982c299)

## Problem Addressed

The `claim` method did not call the `UBIPool.canClaim()` function before submitting a transaction. This meant that users could send a claim transaction that did nothing, unnecessarily consuming gas. It also created ambiguity for developers integrating the SDK, as they had no clear way to determine the wallet’s current claim status.

## Solution

A single method was added to determine the full claim status for a given wallet. This enables developers to make better decisions in their app logic and UI presentation.

### Claim Status Conditions

The method now handles the following conditions:

1. **Not whitelisted**  
   - The wallet is not yet verified. Developers can still show the claim button, but should redirect the user to a verification flow or display an appropriate message.

2. **Can claim (entitlement > 0)**  
   - The user is eligible to claim. Developers should display the claim button.

3. **Already claimed**  
   - The user has claimed their entitlement for the current period. Developers can show a timer or message indicating when the next claim will be available.

In addition, if `checkEntitlement > 0`, the method returns `nextClaimTime = 0` to simplify front-end logic for identifying wallets that are immediately eligible to claim.
