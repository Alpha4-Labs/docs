# Alpha Points Architecture

This directory contains the comprehensive architecture diagrams for the Alpha Points protocol, accurately representing the system as implemented for the investor data room.

## Architecture Files

### 📊 **UML-Compliant Diagrams** (Engineering Standards)
- `alpha-points-uml-architecture.mermaid` - **UML 2.5 Component Diagram** following software engineering standards
- `alpha-points-deployment-diagram.mermaid` - **UML 2.5 Deployment Diagram** showing runtime distribution

### 📈 **Overview Diagrams** (Business/Technical)
- `alpha-points-architecture.mermaid` - High-level 3-tier architecture overview
- `alpha-points-system-architecture.mermaid` - Comprehensive low-level system architecture

### 📋 **Technical Documentation**
- `README.md` - Architecture overview and usage guide
- `tech-stack.md` - Complete technology stack breakdown with exact versions

## UML Architecture Overview

### 🏗️ **Component Diagram** (`alpha-points-uml-architecture.mermaid`)

Following **UML 2.5 standards** and software engineering best practices:

#### **System Structure**
- **<<system>>** Alpha Points Protocol
- **<<layer>>** Clear layer separation with proper interfaces
- **<<subsystem>>** Logical grouping of related components
- **<<component>>** Individual deployable units

#### **Component Types**
- **<<component>>** - Deployable software components
- **<<contract>>** - Smart contract modules
- **<<service>>** - Processing services
- **<<framework>>** - Development frameworks
- **<<library>>** - External libraries
- **<<external>>** - External system dependencies

#### **Layer Architecture**
1. **Presentation Layer**: Client applications and external systems
2. **Application Layer**: Web frameworks, state management, UI components
3. **Integration Layer**: Blockchain integration, transaction processing, event handling
4. **Protocol Layer**: Smart contracts, core business logic, integration points
5. **Blockchain Layer**: Sui platform, runtime, storage
6. **Infrastructure Layer**: Deployment environments, monitoring

### 🚀 **Deployment Diagram** (`alpha-points-deployment-diagram.mermaid`)

Following **UML 2.5 deployment standards**:

#### **Deployment Structure**
- **<<deployment>>** Production environment
- **<<node>>** Hardware/runtime nodes
- **<<execution environment>>** Runtime environments
- **<<device>>** Physical/virtual devices
- **<<artifact>>** Deployable artifacts

#### **Node Types**
- **Client Device**: Web browsers, wallet extensions
- **CDN Network**: Static hosting, SDK distribution
- **Blockchain Network**: Sui mainnet/testnet validators
- **External Services**: Discord, BlockVision, price oracles
- **RPC Infrastructure**: Public and private RPC endpoints
- **Development Environment**: Local development, testing

#### **Artifact Distribution**
- **Static Assets**: Frontend applications distributed via CDN
- **Smart Contracts**: Move contracts deployed across validator nodes
- **SDK**: Zero-dev SDK distributed via CDN
- **State Storage**: Distributed across blockchain nodes with consensus

### 📊 **Engineering Standards Compliance**

#### **UML 2.5 Features**
- **Stereotypes**: Proper use of `<<stereotype>>` notation
- **Interface Definitions**: Clear component interfaces
- **Dependency Relationships**: Explicit dependencies with labeled connections
- **Artifact Management**: Proper artifact and deployment relationships
- **Color Coding**: Consistent visual classification

#### **Software Engineering Best Practices**
- **Separation of Concerns**: Clear layer boundaries
- **Modularity**: Well-defined component boundaries
- **Scalability**: Distributed architecture design
- **Deployment Independence**: Environment-specific configurations
- **Interface Contracts**: Clear API boundaries

### 🔧 **Technical Specifications**

#### **Component Classification**
```
<<component>>    - Frontend Apps, Partners App, SDK
<<contract>>     - Smart contracts (.move files)
<<service>>      - Processing services, APIs
<<framework>>    - React, Vite, Tailwind
<<library>>      - @mysten/sui, @mysten/dapp-kit
<<external>>     - Discord, BlockVision, Price Oracles
<<artifact>>     - Deployable code artifacts
<<database>>     - State storage systems
```

#### **Relationship Types**
```
-->     Direct dependency
-.->    Deployment relationship
<-->    Bidirectional communication
|API|   Interface specification
```

## System Architecture Overview

### 🏗️ **High-Level Architecture** (`alpha-points-architecture.mermaid`)

The original 3-tier architecture overview:

**Client Applications:**
- **Frontend App** (Port 5173): User-facing React/TypeScript application for staking, perk redemption, and point management
- **Partners App** (Port 5174): Dedicated partner dashboard for TVL-backed onboarding and revenue analytics

**Blockchain Infrastructure:**
- **Sui Blockchain**: Decentralized Move smart contracts handling all protocol logic and state management

**External Services:**
- **Price Oracles**: Real-time SUI/USD price feeds for TVL calculations
- **RPC Endpoints**: Blockchain connectivity and transaction processing

### 📊 **Comprehensive System Architecture** (`alpha-points-system-architecture.mermaid`)

This is the most detailed technical architecture diagram showing the complete Alpha Points system with all layers and components:

#### **Tech Stacks**
- **Frontend Stack**: React 18.2.0, TypeScript 5.2.2, Vite 5.4.19, Tailwind CSS 3.4.0
- **Backend Stack**: Sui Move 2024.alpha, @mysten/sui 1.30.1, @mysten/dapp-kit 0.16.5
- **Integration Stack**: BCS Serialization, Transaction Builder, Wallet Connection, Event System

#### **Application Layer**
- **Client Applications**: Frontend (5173), Partners (5174), Rewards, SDK Layer
- **External Integrations**: Discord, BlockVision API, Google OAuth, Enoki SDK, Walrus Storage

#### **User Control Layer**
- **Wallet Management**: @mysten/dapp-kit integration with auto-connect and multi-wallet support
- **User Flows**: Staking, rewards, perks, analytics, loan operations
- **Partner Flows**: TVL deposits, perk creation, quota monitoring, revenue tracking

#### **Transfer Layer**
- **Transaction Processing**: BCS serialization, gas optimization, sponsored transactions
- **Event System**: Auto-detection, rate limiting, spam prevention, quota validation

#### **Protocol Layer**
- **Smart Contracts**: Core contracts (ledger, admin, oracle), Business contracts (partner_flex, perk_manager)
- **Economic Model**: 70/20/10 revenue split, TVL-backed quotas, growth loops

#### **Component Layer**
- **Frontend Components**: Pages, components, hooks for user interface
- **Partner Components**: Dashboard, analytics, generation builder
- **Shared Components**: Reusable UI elements and utilities

#### **Computational Layer**
- **Blockchain Processing**: Sui System, Move VM, consensus, gas calculation
- **Data Processing**: Event indexing, oracle systems, analytics

#### **State Layer**
- **On-Chain State**: Shared objects (ledger, config), owned objects (NFTs, positions)
- **Off-Chain State**: Client state, external data, caching systems

#### **Production & Testing Environments**
- **Deployment**: Mainnet/Testnet with monitoring and analytics
- **Testing**: Development environment with comprehensive testing suites

### 🏭 Mid-Level Architecture

**Frontend System:**
- @mysten/dapp-kit wallet integration
- Staking operations and position management
- Perk marketplace and redemption flow
- Alpha Points balance tracking

**Partner System:**
- TVL-backed partner onboarding
- Perk creation and management tools
- Revenue analytics and quota monitoring
- Collateral management

**Smart Contract Layer (V2/V3 Active):**
- **Core Contracts**: admin_v2.move, ledger_v2.move, oracle_v2.move
- **Business Logic**: generation_manager_v2.move, perk_manager_v2.move, partner_v3.move
- **Integration**: integration_v2.move (user-facing entry points)
- **Legacy**: All `.disabled` modules are V1 implementations (deprecated)

### ⚙️ Low-Level Architecture

**Frontend Components:**
- Pages: Welcome, Dashboard, Marketplace, Analytics, Loan, Generation
- Components: PointsDisplay, StakeCard, PerkCard, LoanPanel, TransactionHistory
- Hooks: useAlphaPoints, useStakePositions, usePerkData, useLoans

**Partner Components:**
- Pages: PartnerOnboarding, Partners, Welcome
- Components: PartnerDashboard, GenerationBuilder, PerkCreationForm, AnalyticsTab
- Hooks: usePartnerOnboarding, usePartnerAnalytics, usePartnerGenerations

**Smart Contract Details (V2/V3 Architecture):**
- **admin_v2.move**: Multi-sig governance, parameter validation, emergency controls
- **ledger_v2.move**: Fixed APY calculations, point accounting, supply tracking  
- **partner_v3.move**: USDC-backed vaults, DeFi integration, collateral management
- **generation_manager_v2.move**: Action registration, quota validation, webhook support
- **perk_manager_v2.move**: USDC redemption marketplace, revenue distribution
- **oracle_v2.move**: Multi-source price feeds (Pyth + CoinGecko), failover logic
- **integration_v2.move**: User-facing entry points, safety checks, simplified flows

**Transaction Flow:**
- Transaction types: Stake SUI, claim rewards, redeem perks, partner operations
- Transaction adapter: @mysten/sui.js integration with gas optimization and error handling

## Key Features (V2/V3 System)

1. **Fixed APY Staking**: Users stake SUI with corrected APY calculations (no more 223x bug)
2. **USDC Perk Marketplace**: Redeem points for USDC-priced perks with revenue sharing
3. **USDC-Backed Partner System**: Stable collateral eliminates volatility risk
4. **Partner Integration Infrastructure**: Action registration, webhook support, quota management
5. **Multi-Source Oracle System**: Pyth + CoinGecko with automatic failover
6. **DeFi Vault Integration**: Partner vaults can be used in yield protocols
7. **Multi-Sig Governance**: Enhanced security with timelock mechanisms

## Technology Stack

- **Frontend**: React 18, TypeScript, Vite, Tailwind CSS
- **Blockchain**: Sui Move smart contracts
- **Wallet Integration**: @mysten/dapp-kit
- **State Management**: React Context API
- **Styling**: Tailwind CSS with custom components

## Implementation Notes

This architecture reflects the **V2/V3 system implementation** with all legacy V1 modules marked as `.disabled`. Key improvements include:

- **Fixed Economics**: Eliminated 223x APY bug with proper basis points calculations
- **USDC Stability**: Replaced volatile SUI collateral with stable USDC backing
- **Enhanced Security**: Multi-sig governance with timelock mechanisms
- **DeFi Ready**: Vault objects designed for protocol integration
- **Production Grade**: Comprehensive testing, error handling, and monitoring

**Migration Status**: All active development uses V2/V3 modules. Legacy `.disabled` modules are kept for reference only.

## Usage

### Viewing the Diagrams

1. **For Engineering Review**: Use UML-compliant diagrams for technical specifications
2. **For Business Overview**: Use high-level architecture for executive summaries
3. **For Technical Deep-Dive**: Use comprehensive system architecture for detailed understanding
4. **For Implementation Details**: Reference tech-stack.md for exact versions and configurations

### Diagram Selection Guide

| Use Case | Recommended Diagram |
|----------|-------------------|
| **Code Review** | UML Component Diagram |
| **Deployment Planning** | UML Deployment Diagram |
| **Executive Summary** | High-Level Architecture |
| **Technical Deep-Dive** | Comprehensive System Architecture |
| **Integration Planning** | Component + Deployment Diagrams |

### Integration with Tools

These `.mermaid` files can be rendered in:
- **GitHub**: Automatically rendered in README files
- **Mermaid Live Editor**: https://mermaid-live-github-com.translate.goog/
- **VS Code**: Mermaid Preview extension
- **Documentation Tools**: GitBook, Notion, Confluence with Mermaid support
- **UML Tools**: Compatible with standard UML visualization tools

## Technology Stack Overview

The Alpha Points protocol is built on a modern, production-ready tech stack:

### **Core Technologies**
- **Blockchain**: Sui Move 2024.alpha edition with comprehensive smart contract architecture
- **Frontend**: React 18 + TypeScript + Vite for modern, fast development
- **Integration**: @mysten/sui SDK v1.30.1 for blockchain connectivity
- **Styling**: Tailwind CSS + Headless UI for consistent, accessible design
- **State Management**: React Query + Context API for efficient data management

### **External Dependencies**
- **Price Oracles**: Real-time SUI/USD conversion rates
- **Event System**: Comprehensive activity tracking and analytics
- **Discord Integration**: Community engagement and notifications
- **CDN Distribution**: SDK deployment at onboard.alpha4.io

This comprehensive architecture documentation provides the foundation for understanding, implementing, and maintaining the Alpha Points protocol's technical infrastructure. 