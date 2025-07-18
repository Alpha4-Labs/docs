# Partner Module Overview

## Module Description
The partner module (sources/partner.move) manages PartnerCap objects for authorized partners, including collateral handling (SUI, USDC, NFT), quotas, oracles, and events. Key structs: PartnerCap, NFTCollateralInfo, USDCCollateralInfo. Public functions include create_partner_cap_with_collateral, calculate_health_factor, etc.

## Security and Audit Notes

### Key Risks
- **Oracle Manipulation**: Stale prices (E_PRICE_TOO_STALE) could lead to incorrect collateral valuation or quota adjustments.
- **Liquidation Vulnerabilities**: Low health factors (< MIN_HEALTH_FACTOR=120%) may allow unfair liquidations; NFT bundles risk value drops.
- **Collateral Theft**: Dynamic fields (e.g., NFTCollateralKey) could be exploited if not properly access-controlled.
- **Quota Abuse**: Unlimited minting if daily_quota_pts not reset properly (via clock/epoch).
- **Reentrancy**: TX flows (e.g., auto_adjust_quota_for_price_change) could be reentered if not atomic.

### Mitigations
- Use assert_not_stale and MAX_PRICE_STALENESS_EPOCHS=24 for oracles.
- Enforce LTV ratios (e.g., NFT_COLLATERAL_LTV_RATIO=70%) and min thresholds (MIN_HEALTH_FACTOR=120%).
- Admin-only functions (e.g., liquidate_nft_collateral) require AdminCap.
- Dynamic fields are upgrade-safe and access-restricted.
- Rate limiting via globalRateLimiter.ts in frontend.

### Audit Recommendations
- Third-party audit for collateral flows (e.g., via Certik); focus on health factor calculations and liquidation thresholds.
- Fuzz testing for edge cases (e.g., zero collateral, max bundle size=50).
- Monitor events (e.g., NFTCollateralLiquidated) in production via BlockVision.
- Best Practice: Always validate inputs (e.g., bundle_size >= MIN_NFT_BUNDLE_SIZE=1) before TX submission.

For error codes, see [errors.md](./errors.md). Source: ```1:1674:sources/partner.move. 