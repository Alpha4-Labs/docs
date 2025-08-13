# Alpha Points API Reference

This is a manual reference for key public functions in the active smart contracts (sources/). All `.disabled` modules are legacy V1 implementations and should not be used.

## admin_v2.move - Protocol Governance

| Function | Params | Returns | Notes |
|----------|--------|---------|-------|
| `update_apy_rate` | config: &mut ConfigV2, admin_cap: &AdminCapV2, new_apy_basis_points: u64, _clock: &Clock, ctx: &mut TxContext | - | Updates APY rate with validation (500 = 5% APY). Emits APYRateUpdated. |
| `update_grace_period` | config: &mut ConfigV2, admin_cap: &AdminCapV2, new_period_ms: u64, _clock: &Clock, ctx: &mut TxContext | - | Updates forfeiture grace period with bounds checking. |
| `set_emergency_pause` | config: &mut ConfigV2, admin_cap: &AdminCapV2, pause_type: u8, new_state: bool, reason: vector<u8>, _clock: &Clock, ctx: &mut TxContext | - | Emergency pause/unpause (1=emergency, 2=mint, 3=redemption, 4=governance). |
| `get_apy_basis_points` | config: &ConfigV2 | u64 | Returns current APY in basis points (500 = 5% APY). |
| `get_points_per_usd` | config: &ConfigV2 | u64 | Returns points per USD conversion rate (1000 points per $1). |
| `is_paused` | config: &ConfigV2 | bool | Returns true if protocol is emergency paused. |

## partner_v3.move - USDC-Backed Partner System

| Function | Params | Returns | Notes |
|----------|--------|---------|-------|
| `create_partner_with_usdc_vault<T>` | usdc_coin: Coin<T>, partner_name: String, ctx: &mut TxContext | - | Creates PartnerCapV3 and PartnerVault with USDC collateral. Min $100 required. |
| `add_usdc_to_vault<T>` | vault: &mut PartnerVault<T>, usdc_coin: Coin<T>, ctx: &mut TxContext | - | Adds USDC collateral to existing vault. Increases backing capacity. |
| `withdraw_usdc_from_vault<T>` | vault: &mut PartnerVault<T>, partner_cap: &PartnerCapV3, amount: u64, ctx: &mut TxContext | Coin<T> | Withdraws unused USDC, respects backing requirements. |
| `record_points_minting` | vault: &mut PartnerVault<T>, points_amount: u64, current_time_ms: u64, ctx: &mut TxContext | - | Records points minting, updates backing requirements. |
| `can_support_points_minting<T>` | vault: &PartnerVault<T>, points_amount: u64 | bool | Checks if vault has sufficient backing for points minting. |
| `get_vault_info<T>` | vault: &PartnerVault<T> | (u64, u64, u64, u64) | Returns (total_balance, available_balance, reserved_backing, total_points_minted). |

## generation_manager_v2.move - Partner Integration Infrastructure

| Function | Params | Returns | Notes |
|----------|--------|---------|-------|
| `register_partner_integration` | registry: &mut IntegrationRegistry, partner_cap: &PartnerCapV3, partner_vault: &PartnerVault, integration_name: String, integration_type: String, webhook_url: Option<String>, clock: &Clock, ctx: &mut TxContext | - | Registers partner integration with API key generation. |
| `register_action` | registry: &mut IntegrationRegistry, integration: &mut PartnerIntegration, partner_cap: &PartnerCapV3, action_name: String, display_name: String, description: String, category: String, points_per_execution: u64, max_daily_executions: Option<u64>, cooldown_period_ms: u64, requires_context_data: bool, webhook_url: Option<String>, clock: &Clock, ctx: &mut TxContext | - | Registers action that can mint points when executed. |
| `execute_registered_action` | registry: &mut IntegrationRegistry, integration: &mut PartnerIntegration, action: &mut RegisteredAction, partner_cap: &PartnerCapV3, partner_vault: &mut PartnerVault, ledger: &mut LedgerV2, user_address: address, context_data: vector<u8>, execution_source: String, clock: &Clock, ctx: &mut TxContext | - | **CORE FUNCTION**: Executes action and mints points to user. |
| `get_integration_info` | integration: &PartnerIntegration | (String, String, bool, bool, u64) | Returns (name, type, active, approved, action_count). |
| `get_action_info` | action: &RegisteredAction | (String, String, String, u64, u64, bool) | Returns (name, display_name, category, points_per_execution, total_executions, active). |

## ledger_v2.move - Fixed Point Accounting

| Function | Params | Returns | Notes |
|----------|--------|---------|-------|
| `mint_points_with_controls` | ledger: &mut LedgerV2, user_address: address, amount: u64, point_type: PointType, context: vector<u8>, clock: &Clock, ctx: &mut TxContext | - | Mints points with daily limits and supply cap validation. |
| `burn_points_for_redemption` | ledger: &mut LedgerV2, user_address: address, amount: u64, ctx: &mut TxContext | - | Burns points for perk redemption, validates sufficient balance. |
| `get_user_balance` | ledger: &LedgerV2, user_address: address | u64 | Returns user's current point balance. |
| `get_total_supply` | ledger: &LedgerV2 | u64 | Returns total points supply across all users. |
| `partner_reward_type` | - | PointType | Returns PointType for partner-generated rewards. |
| `staking_reward_type` | - | PointType | Returns PointType for staking rewards. |

## perk_manager_v2.move - Points Redemption Marketplace

| Function | Params | Returns | Notes |
|----------|--------|---------|-------|
| `create_perk_definition<T>` | partner_cap: &PartnerCapV3, partner_vault: &PartnerVault<T>, name: String, description: String, points_cost: u64, usdc_cost: u64, max_redemptions: Option<u64>, metadata: vector<u8>, clock: &Clock, ctx: &mut TxContext | - | Creates redeemable perk with USDC pricing. |
| `redeem_perk<T>` | perk: &mut PerkDefinition, ledger: &mut LedgerV2, partner_vault: &mut PartnerVault<T>, payment: Coin<T>, user_address: address, clock: &Clock, ctx: &mut TxContext | - | Redeems perk, burns points, processes USDC payment with revenue sharing. |
| `set_perk_active_status` | perk: &mut PerkDefinition, partner_cap: &PartnerCapV3, is_active: bool, ctx: &mut TxContext | - | Activates/deactivates perk for redemption. |
| `get_perk_info` | perk: &PerkDefinition | (String, String, u64, u64, u64, bool) | Returns (name, description, points_cost, usdc_cost, total_redemptions, is_active). |

## oracle_v2.move - Multi-Source Price Feeds

| Function | Params | Returns | Notes |
|----------|--------|---------|-------|
| `get_price_with_failover` | oracle: &OracleV2, clock: &Clock | (u64, u64) | Returns (price, timestamp) with automatic Pyth→CoinGecko failover. |
| `update_pyth_price` | oracle: &mut OracleV2, price_info_object: &PriceInfoObject, clock: &Clock, ctx: &mut TxContext | - | Updates price from Pyth Network feed. |
| `update_coingecko_price` | oracle: &mut OracleV2, new_price: u64, clock: &Clock, ctx: &mut TxContext | - | Updates price from CoinGecko API (backup source). |
| `is_price_fresh` | oracle: &OracleV2, max_staleness_ms: u64, clock: &Clock | bool | Checks if current price is within freshness threshold. |

## integration_v2.move - User-Facing Entry Points

| Function | Params | Returns | Notes |
|----------|--------|---------|-------|
| `stake_sui_and_mint_points` | ledger: &mut LedgerV2, config: &ConfigV2, sui_coin: Coin<SUI>, clock: &Clock, ctx: &mut TxContext | - | Stakes SUI and mints points based on fixed APY calculation. |
| `redeem_points_for_usdc<T>` | ledger: &mut LedgerV2, treasury_vault: &mut PartnerVault<T>, points_amount: u64, clock: &Clock, ctx: &mut TxContext | Coin<T> | Converts points back to USDC at fixed rate (1000 points = $1). |
| `get_user_staking_info` | ledger: &LedgerV2, user_address: address | (u64, u64, u64) | Returns (total_balance, staked_amount, available_for_withdrawal). |

---

## Key Integration Patterns

### For Partners
1. **Setup**: `create_partner_with_usdc_vault()` → `register_partner_integration()` → `register_action()`
2. **Runtime**: `execute_registered_action()` when users complete actions
3. **Revenue**: Earn USDC when users redeem your perks via `redeem_perk()`

### For Users  
1. **Earn Points**: Via `stake_sui_and_mint_points()` or partner actions
2. **Spend Points**: Via `redeem_perk()` for partner offerings
3. **Check Balance**: Via `get_user_balance()`

### For DeFi Protocols
1. **Accept Vaults**: Integrate `PartnerVault<T>` as collateral
2. **Yield Sharing**: 50% to protocol, 50% to partner
3. **Health Monitoring**: Respect backing requirements

---

**Note**: This reference covers the active V2/V3 modules only. All `.disabled` modules are legacy implementations and should not be used in production. For complete function signatures and parameters, refer to the source code in `sources/`. 