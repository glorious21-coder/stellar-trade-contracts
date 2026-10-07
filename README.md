# Stellar Trade Contracts

Soroban contract workspace for Stellar Trade, an open source prediction market on Stellar. The contracts handle XLM market pools, settlement, referrals, participant scores, and a reward asset. Contract resolution is submitted by authorized accounts; there is no automated oracle in this workspace.

## Workspace

- `prediction_market`: market creation, position entry and reduction, resolver actions, proportional claims, cancellations, refunds, fee accounting, and governance.
- `stellar_trade_token`: the Stellar Trade reward asset. Fresh deployments initialize it with the name `Stellar Trade` and symbol `STRD`. Existing deployments keep their original contract address and asset metadata.
- `referral_registry`: registration, bounded referral chains, and referral-fee distribution.
- `leaderboard`: points, wins/losses, ranking, score decay, and reward minting.
- `cross_contract_invariants`: tests for accounting and behavior across the contracts.

The Rust workspace is pinned to 1.91.0 in `rust-toolchain.toml`; install the `wasm32v1-none` target. From this directory:

```bash
cargo test --workspace --all-features
cargo fmt --all -- --check
cargo clippy --workspace --all-features -- -D warnings
cargo build --release --target wasm32v1-none -p prediction_market
```

The public interfaces and safety assumptions are documented in [`INVARIANT_MATRIX.md`](INVARIANT_MATRIX.md). Add regression tests when changing payout calculations, authorization, fee flows, or contract interactions. Testnet deployments and browser integration are documented in the [application README](../stellar-trade/README.md).
