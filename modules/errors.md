# Alpha Points Error Codes and Constants

This document lists key error codes and constants from smart contracts in `sources/`. Extracted from files like partner.move, oracle.move. Use for debugging and integration.

## Error Codes

| Code | Description | Module | Handling Advice |
|------|-------------|--------|-----------------|
| `E_COLLATERAL_VALUE_ZERO` (101) | Collateral value is zero (e.g., invalid SUI/USDC deposit). | partner.move | Check deposit amount; ensure oracle price > 0. |
| `E_EMPTY_NFT_BUNDLE` (102) | NFT bundle size < MIN_NFT_BUNDLE_SIZE (1). | partner.move | Validate bundle has at least 1 NFT before calling create_partner_cap_with_nft_bundle. |
| `E_NFT_VALUATION_FAILED` (104) | NFT valuation error (e.g., no collateral info). | partner.move | Verify NFTCollateralKey exists; integrate with NFTCollectionOracle for prices. |
| `E_HEALTH_FACTOR_TOO_LOW` (106) | Health factor < MIN_HEALTH_FACTOR (120%). | partner.move | Monitor via calculate_health_factor; adjust quotas or liquidate. |
| `E_COLLECTION_NOT_WHITELISTED` (110) | Collection not in whitelist. | partner.move | Call whitelist_collection first; check is_collection_whitelisted. |
| `E_PRICE_TOO_STALE` (111) | Price older than MAX_PRICE_STALENESS_EPOCHS (24). | partner.move | Update via update_floor_price; use assert_not_stale. |
| `E_UNAUTHORIZED_PRICE_FEED` (112) | Caller not authorized for price update. | partner.move | Verify price_authorities table; admin sets via whitelist_collection. |
| `E_FLOOR_PRICE_TOO_LOW` (113) | Floor price < min_floor_price_usdc. | partner.move | Check against CollectionInfo.min_floor_price_usdc. |

## Constants

| Constant | Value | Module | Purpose |
|----------|-------|--------|---------|
| `POINTS_QUOTA_PER_USDC_COLLATERAL_UNIT` | 1000 | partner.move | Daily points per USDC collateral (e.g., quota = value * 1000). |
| `NFT_COLLATERAL_LTV_RATIO` | 7000 (70%) | partner.move | Loan-to-value ratio for NFT collateral. |
| `MIN_HEALTH_FACTOR` | 12000 (120%) | partner.move | Min health factor to avoid liquidation. |
| `LIQUIDATION_THRESHOLD` | 11000 (110%) | partner.move | Threshold for triggering liquidation. |
| `MAX_PRICE_STALENESS_EPOCHS` | 24 | partner.move | Max epochs before price is stale (E_PRICE_TOO_STALE). |
| `MIN_COLLECTION_FLOOR_PRICE` | 100 ($1.00) | partner.move | Min acceptable NFT floor price in USDC. |
| `PRICE_UPDATE_THRESHOLD_BPS` | 500 (5%) | partner.move | Price change % to trigger auto quota adjustment. |
| `MIN_USDC_HEALTH_FACTOR` | 5000 (50%) | partner.move | Min health factor for USDC collateral release. |

For full context, see source files (e.g., ```1:200:sources/partner.move). Report new errors via issues. 