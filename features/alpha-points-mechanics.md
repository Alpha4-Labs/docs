# Alpha Points Mechanics

## Overview

Alpha Points are the core reward tokens of the Alpha Points protocol - a non-transferable point system built on Sui blockchain. This document details the complete mechanics of how Alpha Points are earned, managed, and utilized based on the actual smart contract implementation.

## Core Architecture

### **Ledger System** (`ledger.move`)

The ledger module manages the internal accounting of all Alpha Points through a shared `Ledger` object that tracks:

- **User Balances**: Available and locked points for each user
- **Bad Debt**: Tracking of uncollectable debt per user
- **Point Types**: Categorization of different point earning mechanisms

### **Point Balance Structure**

Each user has a `PointBalance` containing:
- **Available Points**: Freely spendable points
- **Locked Points**: Points temporarily locked for various purposes

```move
public struct PointBalance has key, store {
    id: UID,
    available: u64,    // Spendable points
    locked: u64        // Temporarily locked points
}
```

## Point Types & Sources

### **Point Type Categories**

Alpha Points are categorized into four distinct types:

1. **Staking Points** (`PointType::Staking`)
   - Earned from staking SUI tokens
   - Calculated using APY-based rewards (currently with known math issues)

2. **Generic Rewards** (`PointType::GenericReward`)
   - Partner-minted points for user activities
   - Perk revenue redistribution
   - General ecosystem rewards

3. **Loan Proceeds** (`PointType::LoanProceeds`)
   - Points received when borrowing against staked assets
   - Collateralized lending system

4. **Fee Revenue** (`PointType::FeeRevenue`)
   - Points earned from protocol fee distributions
   - Platform revenue sharing

### **Point Earning Mechanisms**

## 1. **Staking Rewards**

### **Staking Process**
Users stake SUI tokens to earn Alpha Points over time through:

- **Stake Position Creation**: Lock SUI for specified duration
- **Periodic Claims**: Claim accrued points without unstaking
- **Maturity Withdrawal**: Retrieve original SUI after lock period

### **⚠️ Critical Math Issue - Staking Rewards**

**Status**: DOCUMENTED - Cannot fix due to Sui Move upgrade constraints. Will be resolved during next update.

**Problem**: Staking rewards are giving **223x more points than intended**

**Expected vs Actual**:
- **Expected**: 1 SUI at 5% APY for 7 epochs = ~3.14 Alpha Points
- **Actual**: 1 SUI gives 700 Alpha Points (223x multiplier error!)

**Root Cause**:
```move
// Current Wrong Formula (ledger.move):
points = (principal_mist * points_rate * epochs) / 1e9
// For 1 SUI, 7 epochs: (1e9 * 100 * 7) / 1e9 = 700 points ❌

// Correct Formula Should Be:
principal_in_AP = (principal_mist * 3280) / 1e9  // Convert to Alpha Points value  
points = (principal_in_AP * apy_bps * epochs) / (10000 * 365)  // APY-based
// For 1 SUI, 7 epochs: (3280 * 500 * 7) / (10000 * 365) = ~3.14 points ✅
```

**Impact**: Users receiving massive over-rewards, breaking protocol economics

## 2. **Partner Rewards**

### **TVL-Backed Partner System**

Partners can earn quota-based minting rights by:

1. **Collateral Deposit**: Lock SUI/USDC as collateral
2. **Quota Calculation**: Receive minting rights based on TVL
3. **Daily Throttling**: 3% of TVL value per day maximum
4. **Lifetime Limits**: 1,000 points per USDC of collateral

### **Partner Minting Process**

```move
// Modern TVL-backed minting with quota validation
public entry fun earn_points_by_partner_flex(
    user: address,
    pts: u64,
    partner_cap: &mut PartnerCapFlex,
    ledger: &mut Ledger,
    clock: &Clock,
    config: &Config,
    ctx: &mut TxContext
)
```

**Quota Validation**:
- **Daily Quota**: Maximum 3% of partner's TVL value per day
- **Lifetime Quota**: Total cap of 1,000 points per USDC collateral
- **Throttling Reset**: Daily limits reset each epoch

## 3. **Loan System**

### **Collateralized Lending**

Users can borrow Alpha Points against staked assets:

**Loan Parameters**:
- **Maximum LTV**: 70% loan-to-value ratio
- **Collateral**: Staked SUI positions
- **Interest**: Currently 0% (placeholder for future implementation)

**Loan Process**:
1. **Collateral Check**: Verify stake position value
2. **LTV Validation**: Ensure loan doesn't exceed 70% of collateral
3. **Encumbrance**: Lock stake position as collateral
4. **Point Minting**: Issue loan proceeds as Alpha Points

### **Loan Fees**

- **Platform Fee**: 0.1% of loan amount to deployer
- **Partner Fee**: 0.02% to referring partner (if applicable)
- **Net Proceeds**: 99.88% to borrower

## 4. **Perk System Revenue**

### **Perk Claiming Mechanics**

When users claim perks, points are redistributed:

**Revenue Split (70/20/10)**:
- **70% Producer Share**: Direct payment to perk creator
- **20% TVL Reinvestment**: Grows partner's effective TVL
- **10% Platform Fee**: Payment to protocol deployer

### **TVL Growth Loop**

The 20% TVL reinvestment creates a sustainable growth mechanism:
1. **Perk Sales**: User pays Alpha Points for perks
2. **Revenue Split**: 20% automatically reinvested
3. **TVL Growth**: Partner's effective collateral increases
4. **Quota Expansion**: Higher TVL enables larger daily/lifetime quotas

## Balance Management

### **Available vs Locked Points**

**Available Points**:
- Freely spendable for perks and redemptions
- Can be locked for specific purposes
- Subject to bad debt validation

**Locked Points**:
- Temporarily unavailable for spending
- Used for:
  - Loan collateral
  - Hedge positions (planned feature)
  - Escrow arrangements
  - Penalty mechanisms

### **Balance Operations**

**Core Operations**:
```move
// Increase available balance
internal_earn(ledger, user, amount, ctx)

// Decrease available balance  
internal_spend(ledger, user, amount, ctx)

// Move available to locked
internal_lock(ledger, user, amount, ctx)

// Move locked to available
internal_unlock(ledger, user, amount, ctx)
```

## Bad Debt Management

### **Bad Debt Tracking**

The system tracks uncollectable debt per user:

**Bad Debt Sources**:
- Failed loan liquidations
- Insufficient collateral recovery
- Default scenarios

**Bad Debt Operations**:
- **Add Bad Debt**: Record unrecoverable amounts
- **Remove Bad Debt**: Process repayments
- **Query Bad Debt**: Check user's debt status

### **Bad Debt Impact**

Users with bad debt may face:
- Restricted borrowing capabilities
- Limited point earning potential
- Account-level restrictions

## Weight Curve Enforcement

### **Weight Curve Mechanics**

For staking-based rewards, the system enforces a weight curve:

```move
public fun enforce_weight_curve<T: store>(
    stake_opt: &Option<StakePosition<T>>, 
    amount: u64, 
    liq_share: u64, 
    clock_obj: &Clock
)
```

**Weight Calculation**:
- **Stake Duration**: Longer stakes receive higher weights
- **Liquidity Share**: Affects maximum mintable amount
- **Time-based**: Weight increases with stake maturity

**Purpose**: Prevents gaming of rewards system by ensuring points align with actual staking commitment.

## Economic Constants

### **Conversion Rates**

**Alpha Points to USD**:
- **1 USD = 1,000 Alpha Points** (base conversion)
- **SUI Price**: ~$3.28 = 3,280 Alpha Points

**TVL Calculations**:
- **Partner Quota**: 1,000 points per USDC of collateral
- **Daily Throttle**: 3% of TVL value per day
- **LTV Ratios**: 70% for volatile assets, 100% for stablecoins

## Events & Monitoring

### **Point Events**

The system emits events for all point operations:

```move
public struct Earned has copy, drop {
    user: address,
    amount: u64
}

public struct Spent has copy, drop {
    user: address,
    amount: u64
}

public struct Locked has copy, drop {
    user: address,
    amount: u64
}

public struct Unlocked has copy, drop {
    user: address,
    amount: u64
}
```

### **Monitoring & Analytics**

**Key Metrics**:
- Total points in circulation
- Daily minting volume
- Bad debt accumulation
- Partner quota utilization
- Perk redemption rates

## Security & Safeguards

### **Access Control**

**Permission Levels**:
- **Public Functions**: User-facing operations
- **Package Functions**: Internal module access only
- **Admin Functions**: Governance-controlled operations

### **Validation Mechanisms**

**Safety Checks**:
- **Overflow Protection**: Prevents arithmetic overflows
- **Balance Validation**: Ensures sufficient funds before operations
- **Quota Enforcement**: Validates partner minting limits
- **Collateral Verification**: Confirms adequate backing for loans

### **Emergency Controls**

**Pause Mechanisms**:
- **Global Pause**: Admin can halt all operations
- **Partner Pause**: Individual partner suspension
- **Function-specific**: Granular operation control

## Future Enhancements

### **Planned Features**

1. **Volatility Hedge System**
   - Lock points for USDC-pegged hedge tokens
   - Minimum lock periods and redemption controls
   - Vault-backed stability mechanisms

2. **Interest Accrual**
   - Time-based interest on loans
   - Compound interest calculations
   - Dynamic interest rates

3. **Enhanced Weight Curves**
   - More sophisticated staking rewards
   - Liquidity provider incentives
   - Time-decay mechanisms

### **Known Issues & Limitations**

**Current Limitations**:
- **Math Error**: 223x over-rewards in staking (cannot fix due to upgrade constraints)
- **Interest Placeholder**: Loan interest not yet implemented
- **Oracle Dependency**: Price feed reliability critical for quotas
- **Upgrade Constraints**: Sui Move upgrade rules limit structural changes

## Implementation Notes

### **Technical Considerations**

**Data Structures**:
- **Table Storage**: Efficient key-value storage for user balances
- **Dynamic Fields**: Flexible data attachment for extensions
- **Event System**: Comprehensive activity logging

**Performance Optimization**:
- **Batch Operations**: Efficient multi-user processing
- **Gas Optimization**: Minimal computational overhead
- **Storage Efficiency**: Optimized data layout

### **Integration Points**

**External Dependencies**:
- **Oracle System**: Price feeds for conversions
- **Staking Manager**: Native SUI staking integration
- **Partner System**: TVL-backed quota management
- **Perk Manager**: Revenue distribution system

This comprehensive mechanics system provides the foundation for a sustainable, secure, and scalable point-based rewards protocol on Sui blockchain. 