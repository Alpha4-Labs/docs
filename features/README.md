# Alpha Points Features Documentation

This directory contains detailed documentation about the core features and mechanics of the Alpha Points protocol, based on the actual smart contract implementation.

## Feature Documentation

### 📊 [Alpha Points Mechanics](./alpha-points-mechanics.md)
**Comprehensive guide to how Alpha Points work**

Detailed documentation of the core Alpha Points system covering:
- **Point Balance Management**: Available vs locked points, balance operations
- **Point Type Categories**: Staking, rewards, loans, and fee revenue
- **Earning Mechanisms**: Staking rewards, partner minting, loan proceeds, perk revenue
- **Balance Operations**: Earn, spend, lock, unlock functions
- **Bad Debt Management**: Tracking and recovery mechanisms
- **Weight Curve Enforcement**: Anti-gaming measures for staking rewards
- **⚠️ Known Issues**: Critical math error in staking rewards (223x over-rewards)

**Key Topics:**
- Ledger system architecture
- Point earning and spending flows
- Security and validation mechanisms
- Economic constants and conversion rates
- Future enhancements and planned features

### 🔄 [Rewards Engine Model](./rewards-engine-model.md)
**Complete system architecture and reward distribution model**

Comprehensive documentation of the entire rewards engine covering:
- **Partner Ecosystem**: TVL-backed quota system and multi-collateral support
- **Perk System**: Creation, claiming, and revenue distribution (70/20/10 split)
- **Generation Manager**: Point generation opportunities and execution types
- **Zero-Dev Integration**: SDK-based automatic event detection and point minting
- **Loan System**: Collateralized lending with partner attribution
- **Economic Model**: TVL growth loops and sustainable tokenomics

**Key Topics:**
- Partner onboarding and quota management
- Revenue sharing and TVL reinvestment
- Perk marketplace mechanics
- Oracle integration and price feeds
- Analytics and monitoring systems
- Risk management and security measures

## System Overview

### **Architecture Relationship**

The Alpha Points system operates through interconnected components:

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Partner       │    │   Rewards       │    │   Alpha Points  │
│   Ecosystem     │───▶│   Engine        │───▶│   Mechanics     │
│                 │    │                 │    │                 │
│ TVL-Backed      │    │ Perk System     │    │ Balance Mgmt    │
│ Quotas          │    │ Revenue Splits  │    │ Point Types     │
│ Collateral      │    │ Growth Loops    │    │ Operations      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### **Key Features**

#### **🏦 TVL-Backed Partner System**
- **Collateral-based quotas**: Partners deposit SUI/USDC for minting rights
- **Daily throttling**: 3% of TVL value per day maximum
- **Lifetime limits**: 1,000 points per USDC of collateral
- **Growth accommodation**: TVL increases expand quotas automatically

#### **💰 Revenue Sharing (70/20/10)**
- **70% Producer Share**: Direct payment to perk creators
- **20% TVL Reinvestment**: Automatic growth of partner TVL
- **10% Platform Fee**: Protocol operations and development

#### **🔄 Sustainable Growth Loop**
1. Partners deposit collateral → 2. Receive minting quotas → 3. Create valuable perks → 4. Users spend points on perks → 5. Revenue reinvested in TVL → 6. Higher quotas enable more growth

#### **⚡ Zero-Dev Integration**
- **Automatic event detection**: Form submissions, purchases, social shares
- **SDK integration**: Drop-in JavaScript for any website
- **Rate limiting**: Spam prevention and abuse protection
- **Partner attribution**: Revenue sharing for referred activities

#### **🔒 Collateralized Lending**
- **70% LTV ratio**: Borrow against staked SUI positions
- **Partner attribution**: 20% of fees shared with referring partners
- **Flexible collateral**: Support for multiple asset types

### **Critical Issues & Limitations**

#### **⚠️ Known Math Error**
**Status**: Cannot fix due to Sui Move upgrade constraints
- **Problem**: Staking rewards giving 223x more points than intended
- **Impact**: Users receiving massive over-rewards
- **Workaround**: New functions planned for future upgrades

#### **🚧 Placeholder Features**
- **Loan interest**: Currently 0% (placeholder for future implementation)
- **NFT collateral**: Planned but not yet implemented
- **Hedge system**: Volatility protection system in development

### **Economic Model**

#### **Value Proposition**
- **For Users**: Earn Alpha Points through staking, activities, and engagement
- **For Partners**: Generate revenue through perk creation and TVL growth
- **For Protocol**: Sustainable tokenomics with built-in growth mechanisms

#### **Risk Management**
- **Collateral protection**: Multi-asset support with appropriate LTV ratios
- **Quota protection**: Daily throttling prevents rapid dilution
- **Oracle security**: Price feed validation and staleness protection
- **Emergency controls**: Pause mechanisms for system protection

### **Implementation Status**

#### **✅ Production Ready**
- Core ledger and balance management
- Partner onboarding and quota systems
- Perk creation and claiming
- Revenue distribution mechanisms
- Oracle integration and price feeds

#### **🔄 Active Development**
- Enhanced analytics and monitoring
- Volatility hedge system
- Advanced partner features
- Governance and compliance tools

#### **📋 Planned Features**
- Interest-bearing loans
- NFT collateral support
- Automated strategy systems
- Advanced risk management

## Getting Started

### **For Partners**
1. **Review Architecture**: Understand the TVL-backed quota system
2. **Plan Collateral**: Choose appropriate collateral types (SUI/USDC)
3. **Design Perks**: Create valuable rewards for your users
4. **Integrate SDK**: Implement Zero-Dev event detection
5. **Monitor Performance**: Track quota usage and TVL growth

### **For Developers**
1. **Study Mechanics**: Understand point earning and spending flows
2. **Review Smart Contracts**: Examine the actual implementation
3. **Integrate APIs**: Connect to the ledger and partner systems
4. **Implement Events**: Set up proper event tracking and validation
5. **Test Thoroughly**: Validate quota limits and error handling

### **For Users**
1. **Earn Points**: Stake SUI, complete activities, engage with partners
2. **Claim Perks**: Use points to access valuable rewards and benefits
3. **Optimize Strategy**: Understand the different point earning mechanisms
4. **Monitor Balance**: Track available vs locked points
5. **Participate**: Engage with the ecosystem to maximize rewards

## Technical References

### **Smart Contract Modules**
- `ledger.move`: Core point accounting and balance management
- `partner_flex.move`: TVL-backed partner system and quota management
- `perk_manager.move`: Perk creation, claiming, and revenue distribution
- `integration.move`: Main entry points and orchestration
- `loan.move`: Collateralized lending system
- `oracle.move`: Price feeds and conversion functions

### **Key Constants**
- **1 USD = 1,000 Alpha Points**: Base conversion rate
- **SUI Price**: ~$3.28 = 3,280 Alpha Points
- **Partner Quota**: 1,000 points per USDC of collateral
- **Daily Throttle**: 3% of TVL value per day
- **Revenue Split**: 70% producer / 20% TVL growth / 10% platform

### **External Dependencies**
- **Sui Blockchain**: Layer 1 blockchain platform
- **Price Oracles**: Real-time SUI/USD price feeds
- **Wallet Integration**: @mysten/dapp-kit for user interactions
- **Event System**: Comprehensive activity tracking and analytics

This documentation provides the foundation for understanding and implementing the Alpha Points protocol's comprehensive reward system architecture. 