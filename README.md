# open-source-contributions

## Contribution to GoodDollar SDK

**Commit:** [`7f1d8c8`](https://github.com/GoodDollar/GoodSDKs/commit/7f1d8c8f9698a84ef1a000f38c2d7818f982c299)  
**File modified:** `citizen-sdk/src/sdks/viem-claim-sdk.ts`  
**Lines changed:** +66 / -7

## Overview

This contribution improves the logic around claiming UBI in the GoodDollar SDK by introducing:

- A pre-claim eligibility check
- A unified claim status method
- A more intelligent `nextClaimTime` function

These changes prevent unnecessary or no-op claim transactions and help developers handle user states in a predictable and gas-efficient way.

---

## Key Changes

### 1. **Added `getWalletClaimStatus()` Method**

Introduced a new method that returns a complete status object (`WalletClaimStatus`) for a given wallet address, including:

- `status`: One of `"not_whitelisted"`, `"can_claim"`, or `"already_claimed"`
- `entitlement`: The current UBI entitlement (as a `bigint`)
- `nextClaimTime` (optional): When the user can next claim (only returned if status is `already_claimed`)

This provides developers a single entry point to determine claim status and update UI/UX accordingly.

### 2. **Updated `claim()` Logic**

Previously, the `claim()` method did not check whether the user had any claimable UBI, which could lead to wasted gas. Now it:

- Verifies that the user is whitelisted
- Calls `checkEntitlement()` before proceeding
- Aborts early if `entitlement === 0n` with a descriptive error

This avoids sending claim transactions when nothing can be claimed.

### 3. **Improved `nextClaimTime()` Handling**

The `nextClaimTime()` method now:

- Checks if `entitlement > 0n` and returns `Date(0)` (epoch) if the user can already claim
- Otherwise calculates and returns the actual next eligible claim date

This makes the method safer and aligns it with real-time entitlement logic.

---
