# [MEDIUM] legacy reward asset token has no approve/allowance mechanism — cannot integrate with DeFi or delegated spending

## Summary

The legacy reward asset token (`stellar_trade_token/src/lib.rs`) implements only `transfer`, `burn`, and `mint`. There is **no `approve`/`allowance`/`transfer_from`** mechanism. This means:

- No third-party contract can spend legacy reward asset on a user's behalf (no delegated transfers).
- The token cannot integrate with standard DeFi primitives (DEXs, lending, staking) that require allowances.
- Users must transfer legacy reward asset directly, which is incompatible with most on-chain applications.

This is a **functional limitation** that makes the token non-composable with the broader Stellar/Soroban ecosystem.

## Why it's unsolvable by a localized patch

- **Adding `approve`/`allowance`/`transfer_from`** requires new storage keys, new functions, and a new ABI — a feature addition, not a bug fix.
- **The token is already deployed** (in the test/deploy flow), so adding these functions requires an upgrade (issue #5) and a migration.
- **A correct fix** requires implementing the full allowance model and deciding the approval semantics — a token redesign.

The root problem is **the token is not ERC-20/SAC-compatible**. A correct fix is a feature/design addition.