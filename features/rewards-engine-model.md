# Rewards Engine Model

## Overview

The Alpha Points Rewards Engine is a comprehensive blockchain-based system that enables partners to create sustainable reward mechanisms for users through TVL-backed quotas, automated revenue sharing, and dynamic growth loops. This document details the complete rewards engine model based on the actual smart contract implementation.

## System Architecture

### **Core Components**

1. **Partner System** (`partner_flex.move`) - TVL-backed quota management
2. **Perk Manager** (`perk_manager.move`) - Reward creation and distribution
3. **Generation Manager** (`generation_manager.move`) - Point generation opportunities
4. **Ledger System** (`ledger.move`) - Point accounting and balance management
5. **Oracle System** (`oracle.move`) - Price feeds and conversions
6. **Loan System** (`loan.move`) - Collateralized lending mechanisms

### **Data Flow Architecture**

```
Partner Collateral → TVL Calculation → Quota Allocation → Point Minting → Perk Creation → Revenue Distribution → TVL Growth → Quota Expansion
```

## Partner Ecosystem

### **TVL-Backed Partner System**

#### **Partner Onboarding Process**

1. **Collateral Deposit**
   ```move
   public entry fun create_partner_cap_flex_with_collateral(
       sui_collateral_coin: Coin<SUI>,
       rate_oracle: &RateOracle,
       partner_name: String,
       ctx: &mut TxContext
   )
   ```

2. **Oracle Valuation**
   - SUI/USD price fetched from oracle
   - Collateral value calculated in USDC terms
   - LTV ratios applied based on asset type

3. **Quota Calculation**
   - **Lifetime Quota**: `TVL × 1,000 Alpha Points`
   - **Daily Quota**: `3% of TVL value per day`
   - **Throttling Protection**: Prevents rapid dilution

4. **NFT Issuance**
   - `PartnerCapFlex` NFT minted with quota tracking
   - Collateral locked in shared `CollateralVault`
   - Dynamic fields for extensibility

#### **Multi-Collateral Support**

**Supported Collateral Types**:

1. **SUI Collateral** (Primary)
   - Native blockchain token
   - Oracle-based USD valuation
   - Moderate LTV ratios

2. **USDC Collateral** (Stable)
   - 1:1 USD valuation
   - 100% LTV (no conversion risk)
   - Instant quota activation

3. **NFT Collateral** (Planned)
   - Floor price estimation
   - 70% LTV for volatility protection
   - Kiosk-based custody

#### **Quota Management**

**Daily Throttling System**:
```move
const DAILY_MINT_THROTTLE_PERCENT_BASIS_POINTS: u64 = 300; // 3.00%
```

**Quota Validation**:
- **Daily Limits**: Resets each epoch
- **Lifetime Limits**: Total cap based on collateral
- **Growth Accommodation**: TVL increases expand quotas
- **Validation Layer**: Every mint operation checked

### **Partner Revenue Model**

#### **Revenue Streams**

1. **Direct Partner Fees**
   - 20% of loan origination fees
   - Perk creation commissions
   - Zero-dev integration fees

2. **TVL Growth Reinvestment**
   - 20% of perk revenue automatically reinvested
   - Increases effective TVL
   - Expands future quota capacity

3. **Revenue Attribution**
   - Partner-specific fee tracking
   - Attribution events for analytics
   - Performance-based incentives

## Perk System

### **Perk Creation & Management**

#### **Perk Definition Structure**

```move
public struct PerkDefinition has key {
    id: UID,
    creator: address,
    name: String,
    description: String,
    current_alpha_points_price: u64,
    max_claims: Option<u64>,
    total_claims_count: u64,
    is_active: bool,
    revenue_split_policy: RevenueSplitPolicy,
    // ... additional fields
}
```

#### **Revenue Split Models**

**Enhanced Revenue Model (70/20/10)**:
- **70% Producer Share**: Direct payment to perk creator
- **20% TVL Reinvestment**: Grows partner's effective TVL
- **10% Platform Fee**: Payment to protocol deployer

**Configurable Revenue Split**:
- Partners can set custom revenue percentages
- Flexible distribution policies
- Dynamic adjustment capabilities

### **Perk Claiming Process**

#### **Standard Perk Claiming**

1. **Validation Phase**
   - Verify perk is active
   - Check maximum claims limit
   - Validate user point balance

2. **Payment Processing**
   - Spend user's Alpha Points
   - Calculate revenue splits
   - Apply quota validations

3. **Revenue Distribution**
   ```move
   // Calculate splits
   let producer_share = cost * 70 / 100;
   let tvl_reinvestment_share = cost * 20 / 100;
   let deployer_share = cost - producer_share - tvl_reinvestment_share;
   
   // Distribute revenue
   ledger::internal_earn(ledger, partner_address, producer_share, ctx);
   ledger::internal_earn(ledger, deployer_address, deployer_share, ctx);
   
   // Reinvest in TVL
   partner_flex::reinvest_perk_revenue_alpha_points(partner_cap, tvl_reinvestment_share, oracle, ctx);
   ```

4. **Claimed Perk Creation**
   - Generate unique claimed perk NFT
   - Store claim metadata
   - Track usage and status

#### **Advanced Perk Features**

**Metadata Support**:
- Custom key-value metadata storage
- Claim-specific information
- Dynamic field extensions

**Usage Tracking**:
- Multi-use perks supported
- Usage counters per claim
- Status management (ACTIVE/USED/EXPIRED)

**Price Dynamics**:
- Fixed pricing models
- Time-based price updates
- Market-responsive adjustments

## Generation Manager

### **Point Generation System**

#### **Generation Opportunity Structure**

```move
public struct Generation has key {
    id: UID,
    creator: address,
    name: String,
    description: String,
    execution_type: u8,
    points_per_execution: u64,
    max_executions_per_user: Option<u64>,
    total_executions_limit: Option<u64>,
    // ... additional fields
}
```

#### **Execution Types**

1. **Embedded Code Execution** (`EXECUTION_TYPE_EMBEDDED_CODE`)
   - JavaScript code stored on-chain
   - Walrus blob storage integration
   - Sandboxed execution environment

2. **External URL Redirect** (`EXECUTION_TYPE_EXTERNAL_URL`)
   - Redirect to external applications
   - Completion verification mechanisms
   - Partner-hosted experiences

3. **Hybrid Execution** (`EXECUTION_TYPE_HYBRID`)
   - Combination of embedded and external
   - Multi-step completion flows
   - Complex interaction patterns

#### **Safety & Security**

**Safety Score System**:
- Minimum safety score: 70/100
- Code analysis and validation
- Community-based reputation system

**Execution Limits**:
- Maximum execution time: 30 seconds
- Per-user execution limits
- Total execution caps per generation

**Quota Integration**:
- Partner quota validation
- TVL-backed minting limits
- Daily throttling enforcement

## Zero-Dev Integration

### **SDK Integration System**

#### **Event-Based Point Minting**

```move
public entry fun submit_zero_dev_event(
    partner_cap: &mut PartnerCapFlex,
    ledger: &mut Ledger,
    user_address: address,
    event_type: String,
    event_data: vector<u8>,
    points_per_event: u64,
    clock: &Clock,
    ctx: &mut TxContext
)
```

#### **Automatic Event Detection**

**Event Types**:
- `user_signup`: New user registration
- `purchase_completed`: E-commerce transactions
- `newsletter_signup`: Email subscriptions
- `social_share`: Social media interactions
- `profile_completed`: User profile updates
- `referral_successful`: Referral completions

**Event Processing**:
1. **Validation**: Verify event authenticity
2. **Rate Limiting**: Prevent spam and abuse
3. **Quota Check**: Validate partner minting limits
4. **Point Minting**: Award points to user
5. **Event Recording**: Store event history

#### **Rate Limiting & Spam Prevention**

**Rate Limiting System**:
- Maximum events per hour per user
- Cooldown periods per event type
- Replay attack protection

**Event Validation**:
- Cryptographic event hashing
- Timestamp validation
- Partner authentication

## Loan System Integration

### **Collateralized Lending**

#### **Loan Parameters**

```move
public struct LoanConfig has key {
    id: UID,
    max_ltv_bps: u64,         // Maximum 70% LTV
    interest_rate_bps: u64    // Currently 0% (placeholder)
}
```

#### **Loan Process Flow**

1. **Collateral Validation**
   - Verify stake position ownership
   - Check encumbrance status
   - Calculate collateral value

2. **LTV Calculation**
   - Oracle-based asset valuation
   - Maximum 70% loan-to-value ratio
   - Safety margin enforcement

3. **Quota Integration**
   - Partner quota validation for TVL-backed loans
   - Daily throttling enforcement
   - Lifetime limit checks

4. **Point Minting**
   - Mint loan proceeds as Alpha Points
   - Fee distribution (platform + partner)
   - Stake position encumbrance

#### **Loan Fees & Revenue**

**Fee Structure**:
- **Platform Fee**: 0.1% of loan amount
- **Partner Fee**: 0.02% for referred loans
- **Net Proceeds**: ~99.88% to borrower

**Revenue Attribution**:
- Partner fees tracked and distributed
- Platform fees support protocol operations
- Fee events emitted for analytics

## Economic Model

### **TVL Growth Loop**

#### **Sustainable Growth Mechanism**

1. **Initial Collateral**: Partner deposits SUI/USDC
2. **Quota Allocation**: Minting rights based on TVL
3. **Point Generation**: Partners mint points for users
4. **Perk Creation**: Partners create valuable perks
5. **Revenue Generation**: Users spend points on perks
6. **TVL Reinvestment**: 20% of revenue grows partner TVL
7. **Quota Expansion**: Higher TVL enables more minting
8. **Positive Feedback**: Success breeds more success

#### **Economic Constants**

**Conversion Rates**:
- **1 USD = 1,000 Alpha Points**
- **SUI Price**: ~$3.28 = 3,280 Alpha Points
- **Partner Quota**: 1,000 points per USDC of collateral

**Revenue Splits**:
- **70% Producer Share**: Direct creator payment
- **20% TVL Growth**: Automatic reinvestment
- **10% Platform Fee**: Protocol operations

**Throttling Parameters**:
- **Daily Limit**: 3% of TVL value per day
- **Lifetime Limit**: 1,000 points per USDC collateral
- **Growth Accommodation**: TVL increases expand limits

### **Risk Management**

#### **Collateral Protection**

**LTV Ratios**:
- **SUI**: Dynamic based on volatility
- **USDC**: 100% (stable asset)
- **NFTs**: 70% (volatility protection)

**Liquidation Mechanisms**:
- Health factor monitoring
- Automatic liquidation triggers
- Bad debt tracking and recovery

#### **Quota Protection**

**Daily Throttling**:
- Prevents rapid TVL dilution
- Maintains point value stability
- Allows sustainable growth

**Lifetime Limits**:
- Total cap based on collateral
- Prevents unlimited inflation
- Ensures backing remains adequate

## Oracle Integration

### **Price Feed System**

#### **Oracle Structure**

```move
public struct RateOracle has key {
    id: UID,
    sui_price_usdc: u64,
    alpha_points_per_usdc: u64,
    last_update_epoch: u64,
    // ... additional fields
}
```

#### **Price Conversion Functions**

**SUI to USDC Conversion**:
```move
public fun price_in_usdc(oracle: &RateOracle, sui_amount: u64): u64
```

**Alpha Points Conversion**:
```move
public fun convert_points_to_asset(points: u64, rate: u64, decimals: u8): u64
public fun convert_asset_to_points(asset: u64, rate: u64, decimals: u8): u64
```

#### **Oracle Security**

**Update Mechanisms**:
- Admin-controlled price updates
- Staleness protection
- Deviation limits

**Validation Systems**:
- Price sanity checks
- Update frequency limits
- Emergency pause capabilities

## Analytics & Monitoring

### **Key Performance Indicators**

#### **Partner Metrics**

**TVL Metrics**:
- Total Value Locked per partner
- TVL growth rate over time
- Collateral composition analysis

**Quota Utilization**:
- Daily quota usage rates
- Lifetime quota consumption
- Throttling effectiveness

**Revenue Performance**:
- Partner fee generation
- Perk sales volume
- TVL reinvestment impact

#### **System-Wide Metrics**

**Point Economics**:
- Total points in circulation
- Daily minting volume
- Point redemption rates

**Perk Marketplace**:
- Most popular perks
- Average perk prices
- Revenue distribution patterns

**User Engagement**:
- Active user count
- Point earning activities
- Perk claiming frequency

### **Event System**

#### **Comprehensive Event Tracking**

**Partner Events**:
- TVL deposits and withdrawals
- Quota resets and adjustments
- Revenue distributions

**Point Events**:
- Minting and burning operations
- Balance changes (earn/spend/lock/unlock)
- Bad debt modifications

**Perk Events**:
- Perk creation and updates
- Claim transactions
- Revenue split distributions

**System Events**:
- Oracle price updates
- Admin configuration changes
- Emergency pause activations

## Future Enhancements

### **Planned Features**

#### **Volatility Hedge System**

**Hedge Vault Structure**:
```move
public struct HedgeVault has key {
    id: UID,
    total_usdc: u64,
    available_quota_pts: u64,
    paused: bool
}
```

**Hedge Mechanics**:
- Lock Alpha Points for USDC-pegged hedge tokens
- Minimum lock periods (e.g., 7 days)
- Redemption controls and limits
- Vault-backed stability mechanisms

#### **Advanced Partner Features**

**Multi-Asset Collateral**:
- Diversified collateral portfolios
- Dynamic LTV adjustments
- Cross-asset hedging mechanisms

**Automated Strategies**:
- Auto-reinvestment protocols
- Yield optimization systems
- Risk management automation

#### **Enhanced Analytics**

**Predictive Analytics**:
- TVL growth forecasting
- User behavior modeling
- Revenue optimization insights

**Real-time Monitoring**:
- Live dashboard systems
- Alert mechanisms
- Performance tracking

### **Scaling Considerations**

#### **Performance Optimization**

**Batch Processing**:
- Multi-user operations
- Gas optimization techniques
- Storage efficiency improvements

**State Management**:
- Efficient data structures
- Minimal storage overhead
- Optimized query patterns

#### **Governance Evolution**

**Decentralized Governance**:
- Community-driven parameters
- Stakeholder voting mechanisms
- Automated policy adjustments

**Regulatory Compliance**:
- KYC/AML integration possibilities
- Regulatory reporting systems
- Compliance automation tools

## Implementation Guidelines

### **Integration Best Practices**

#### **Partner Integration**

1. **Collateral Strategy**: Choose appropriate collateral types
2. **Quota Management**: Monitor and optimize quota usage
3. **Revenue Optimization**: Create valuable perks for users
4. **TVL Growth**: Leverage reinvestment mechanisms

#### **Developer Integration**

1. **SDK Usage**: Implement Zero-Dev event detection
2. **Event Validation**: Ensure proper event authentication
3. **Error Handling**: Implement comprehensive error recovery
4. **Monitoring**: Track quota usage and performance

### **Security Considerations**

#### **Smart Contract Security**

**Access Control**:
- Role-based permissions
- Multi-signature requirements
- Time-locked operations

**Validation Systems**:
- Input sanitization
- Overflow protection
- Reentrancy prevention

#### **Economic Security**

**Manipulation Prevention**:
- Oracle price validation
- Rate limiting mechanisms
- Collateral monitoring

**Sustainability Measures**:
- TVL backing requirements
- Growth limit enforcement
- Emergency pause capabilities

This comprehensive rewards engine model provides a robust, scalable, and sustainable framework for partner-driven reward systems on the Sui blockchain, combining innovative tokenomics with practical business incentives. 