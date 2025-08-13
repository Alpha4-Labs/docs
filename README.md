# Alpha Points Documentation

This directory contains comprehensive documentation for the Alpha Points protocol, including system architecture, feature documentation, and technical specifications for the investor data room.

## Documentation Structure

### 🏗️ [Architecture](./architecture/)
**System architecture diagrams and technology stack**

- **[System Architecture Diagram](./architecture/alpha-points-architecture.mermaid)**: Complete Mermaid diagram with High-Level, Mid-Level, and Low-Level architecture views
- **[Technology Stack](./architecture/tech-stack.md)**: Comprehensive breakdown of all technologies, versions, and dependencies across the entire system
- **[Architecture Overview](./architecture/README.md)**: Detailed explanation of the three-tier architecture and key technologies

### 📊 [Features](./features/)
**Detailed feature documentation and system mechanics**

- **[Alpha Points Mechanics](./features/alpha-points-mechanics.md)**: Complete guide to how Alpha Points work, including balance management, point types, and earning mechanisms
- **[Rewards Engine Model](./features/rewards-engine-model.md)**: Comprehensive documentation of the entire rewards engine architecture and distribution model
- **[Features Overview](./features/README.md)**: Summary of all features with getting started guides

## Quick Navigation

### **For Investors**
- **[Architecture Overview](./architecture/README.md)**: High-level system architecture and technology choices
- **[Technology Stack](./architecture/tech-stack.md)**: Complete technical foundation and scalability considerations
- **[Features Summary](./features/README.md)**: Key features and value propositions

### **For Partners**
- **[Rewards Engine Model](./features/rewards-engine-model.md)**: Partner ecosystem and revenue sharing (70/20/10 split)
- **[TVL-Backed Quotas](./features/alpha-points-mechanics.md#partner-rewards)**: Collateral-based minting rights and quota management
- **[Partner Integration Guide](./features/README.md#for-partners)**: Getting started with partner onboarding

### **For Developers**
- **[Alpha Points Mechanics](./features/alpha-points-mechanics.md)**: Technical details of point earning, spending, and balance management
- **[Smart Contract Architecture](./architecture/alpha-points-architecture.mermaid)**: Complete system component relationships
- **[Integration Guide](./features/README.md#for-developers)**: SDK integration and development setup

### **For Users**
- **[User Guide](./features/README.md#for-users)**: How to earn points, claim perks, and optimize strategy
- **[System Overview](./features/README.md#system-overview)**: Understanding the Alpha Points ecosystem

## System Overview

### **What is Alpha Points?**

Alpha Points is a comprehensive blockchain-based rewards protocol built on Sui, enabling partners to create sustainable reward mechanisms through TVL-backed quotas, automated revenue sharing, and dynamic growth loops.

### **Key Features**

#### **🏦 TVL-Backed Partner System**
- Partners deposit SUI/USDC as collateral
- Receive minting quotas based on Total Value Locked (TVL)
- Daily throttling: 3% of TVL value per day maximum
- Lifetime limits: 1,000 points per USDC of collateral

#### **💰 Revenue Sharing (70/20/10)**
- **70% Producer Share**: Direct payment to perk creators
- **20% TVL Reinvestment**: Automatic growth of partner TVL
- **10% Platform Fee**: Protocol operations and development

#### **🔄 Sustainable Growth Loop**
Revenue from perk sales automatically reinvests in partner TVL, creating a positive feedback loop that expands quota capacity over time.

#### **⚡ Zero-Dev Integration**
Drop-in JavaScript SDK for automatic event detection and point minting with comprehensive rate limiting and abuse protection.

#### **🔒 Collateralized Lending**
Users can borrow Alpha Points against staked SUI positions with 70% LTV ratio and partner attribution for fees.

### **Architecture Highlights**

#### **Three-Tier Architecture**
1. **High-Level**: Client applications, blockchain infrastructure, external services
2. **Mid-Level**: Frontend systems, partner systems, smart contract layers
3. **Low-Level**: Detailed components, hooks, contract specifics, transaction flows

#### **Modern Tech Stack**
- **Frontend**: React 18 + TypeScript + Vite
- **Blockchain**: Sui Move smart contracts (2024.alpha edition)
- **Integration**: @mysten/sui SDK v1.30.1
- **Styling**: Tailwind CSS + Headless UI
- **State Management**: React Query + Context API

#### **Smart Contract Modules (Active V2/V3)**
- `admin_v2.move`: Protocol governance with multi-sig and timelock controls
- `ledger_v2.move`: Fixed point accounting with proper APY calculations
- `partner_v3.move`: USDC-backed partner vaults with DeFi integration
- `generation_manager_v2.move`: Partner action registration and quota management
- `perk_manager_v2.move`: Points redemption marketplace with USDC payouts
- `oracle_v2.move`: Multi-source price feeds (Pyth + CoinGecko)
- `integration_v2.move`: User-facing entry points and safety checks

### **Economic Model**

#### **Value Alignment**
- **Users**: Earn points through staking, activities, and engagement
- **Partners**: Generate revenue through perk creation and TVL growth
- **Protocol**: Sustainable tokenomics with built-in growth mechanisms

#### **Risk Management**
- Collateral protection with appropriate LTV ratios
- Daily throttling prevents rapid dilution
- Oracle security with price feed validation
- Emergency controls and pause mechanisms

### **Implementation Status**

#### **✅ Production Ready**
- Core ledger and balance management
- Partner onboarding and quota systems
- Perk creation and claiming mechanisms
- Revenue distribution and TVL growth
- Oracle integration and price feeds

#### **⚠️ Known Issues**
- **Math Error**: Staking rewards giving 223x more points than intended (cannot fix due to Sui Move upgrade constraints)
- **Placeholder Features**: Loan interest (0%), NFT collateral (planned), hedge system (in development)

#### **🔄 Active Development**
- Enhanced analytics and monitoring
- Volatility hedge system
- Advanced partner features
- Governance and compliance tools

## Getting Started

### **Documentation Path**

1. **Start with Architecture**: Understand the system design and technology choices
2. **Review Features**: Deep dive into specific mechanics and capabilities
3. **Choose Your Role**: Follow role-specific getting started guides
4. **Implement**: Use technical references and integration guides

### **Key Resources**

- **[Mermaid Architecture Diagram](./architecture/alpha-points-architecture.mermaid)**: Visual system overview
- **[Technology Stack Breakdown](./architecture/tech-stack.md)**: Complete technical specifications
- **[Alpha Points Mechanics](./features/alpha-points-mechanics.md)**: Core system functionality
- **[Rewards Engine Model](./features/rewards-engine-model.md)**: Complete business logic

### **Technical Constants**

- **1 USD = 1,000 Alpha Points**: Base conversion rate
- **SUI Price**: ~$3.28 = 3,280 Alpha Points
- **Partner Quota**: 1,000 points per USDC of collateral
- **Daily Throttle**: 3% of TVL value per day
- **Revenue Split**: 70% producer / 20% TVL growth / 10% platform

## Contributing

### **Documentation Updates**

When updating documentation:
1. Ensure accuracy against actual implementation
2. Update both architecture and feature docs if system changes
3. Keep technical constants and versions current
4. Test all code examples and configurations

### **Code References**

All documentation is based on actual smart contract implementation. Key reference files:
- `sources/admin_v2.move`: Protocol governance and configuration management
- `sources/ledger_v2.move`: Fixed point mechanics and balance management
- `sources/partner_v3.move`: USDC-backed partner system with DeFi integration
- `sources/generation_manager_v2.move`: Partner integration infrastructure
- `sources/perk_manager_v2.move`: Points redemption and revenue distribution
- `sources/oracle_v2.move`: Multi-source price feeds and validation
- `sources/integration_v2.move`: User-facing entry points and safety checks

**Note**: All `.disabled` modules are legacy V1 implementations kept for reference only.

This documentation provides comprehensive coverage of the Alpha Points protocol for investors, partners, developers, and users alike. 