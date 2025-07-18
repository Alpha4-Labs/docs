# Alpha Points API Reference

This is a manual reference for key public functions in smart contracts (sources/). For full source, see ```1:END:sources/partner.move.

## partner.move

| Function | Params | Returns | Notes |
|----------|--------|---------|-------|
| `create_partner_cap_with_collateral` | sui_collateral: Coin<SUI>, rate_oracle: &RateOracle, partner_name: String, ctx: &mut TxContext | (transfers PartnerCap) | Creates PartnerCap with SUI collateral; burns SUI; emits PartnerCapCreatedWithCollateral. |
| `create_partner_cap_with_usdc_collateral<USDCType>` | usdc_collateral: Coin<USDCType>, partner_name: String, ctx: &mut TxContext | (transfers PartnerCap) | Creates with USDC (1:1 LTV); stores in dynamic field. |
| `create_partner_cap_with_nft_bundle` | kiosk: &mut Kiosk, kiosk_owner_cap: &KioskOwnerCap, nft_ids: vector<ID>, collection_type: String, estimated_floor_value_usdc: u64, rate_oracle: &RateOracle, partner_name: String, ctx: &mut TxContext | (transfers PartnerCap) | Creates with NFT bundle; locks in kiosk; 70% LTV. |
| `calculate_health_factor` | cap: &PartnerCap | u64 (health factor in bps) | Computes collateral coverage; returns 10000 for 100%. |
| `liquidate_nft_collateral` | admin_cap: &AdminCap, cap: &mut PartnerCap, new_estimated_value_usdc: u64, liquidator: address, ctx: &mut TxContext | (emits NFTCollateralLiquidated) | Admin-only; reduces quotas on low health. |

## oracle.move (Sample)

| Function | Params | Returns | Notes |
|----------|--------|---------|-------|
| `get_rate` | oracle: &RateOracle | (u128, u8) | Returns current rate and decimals; check staleness first. |
| `assert_not_stale` | oracle: &RateOracle, clock: &Clock, ctx: &TxContext | (asserts) | Ensures price not older than max staleness. |

For more, extract from sources/ or use Move tools. Report gaps via issues. 