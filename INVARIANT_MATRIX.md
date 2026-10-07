# Stellar Trade Contract Invariants

This matrix records cross-contract rules that must remain true when market fees, reward values, referral behavior, or storage lifetimes change. The test names below are implemented in `cross_contract_invariants/src/lib.rs` unless otherwise noted.

## Current behavior

| Area | Contract rule | Verification |
| --- | --- | --- |
| Bet fees | 98% of gross stake is credited to the selected side; total fees equal gross minus net. Platform and referral portions are accounted for separately. | `test_inv_cross_fee_conservation`, `test_inv_cross_accumulator_consistency` |
| Pool payout | At resolution, each winning account receives its proportional share. Integer remainder is tracked as market fees; payouts cannot exceed the pool. | `test_inv_cross_fee_conservation` |
| Cancellation | Cancellation refunds eligible gross stakes and reclaims only the cancelled market's fees. | `prediction_market::tests::test_inv_fee_conservation` |
| Empty winning side | Net principal is claimable; fee funds remain protected through the dispute period. | `test_inv_cross_dispute_window_ttl` |
| Fee withdrawals | Each withdrawal is capped; one market's accounting cannot spend another market's fees. | `test_inv_cross_withdraw_cap`, `test_inv_cross_multi_market_isolation` |
| Referral fees | A registered referrer can receive its allocated fee. Missing referrals route the allocation back according to contract rules. Referral depth is bounded. | `test_inv_cross_referral_fee_payment`, `test_inv_cross_referral_depth_isolation`, `test_inv_cross_late_referrer_registration` |
| Rewards and points | Settled outcomes update leaderboard records and mint rewards through authorized contract calls. | `test_inv_cross_reward_asset_minting_bounded`, `test_inv_cross_leaderboard_points` |
| Emergency pause | Paused contract paths reject actions as configured. | `test_inv_cross_emergency_pause_circuit_breaker` |

## Review when changing constants

- Fee rates or withdrawal caps: test fee decomposition, cancellation, and interleaved withdrawals.
- Market durations, dispute windows, or storage TTLs: test create, bet, resolve, dispute, claim, and later reads.
- Reward amounts or supply caps: test total supply and all reward entry points.
- Referral depth or fee allocation: test deep chains, late registration, and fee-ledger isolation.

Run the complete suite from this directory with `cargo test --workspace --all-features`. Keep the contract behavior, these invariants, and the user-facing description in sync.
