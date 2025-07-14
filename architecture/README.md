# Alpha Points Architecture

This directory contains the comprehensive architecture diagrams for the Alpha Points protocol, accurately representing the system as implemented for the investor data room.

## Architecture Files

- `alpha-points-architecture.mermaid` - Complete system architecture diagram (3-tier overview)
- `alpha-points-system-architecture.mermaid` - **NEW** Comprehensive low-level system architecture
- `tech-stack.md` - Comprehensive technology stack breakdown

## System Architecture Overview

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

**Smart Contract Layer:**
- **Core Contracts**: Ledger, Staking Manager, Oracle, Admin
- **Business Logic**: Generation Manager, Perk Manager, Partner Manager, Loan System
- **Support Contracts**: Escrow, Stake Position, Pending Withdrawals, Integration

### ⚙️ Low-Level Architecture

**Frontend Components:**
- Pages: Welcome, Dashboard, Marketplace, Analytics, Loan, Generation
- Components: PointsDisplay, StakeCard, PerkCard, LoanPanel, TransactionHistory
- Hooks: useAlphaPoints, useStakePositions, usePerkData, useLoans

**Partner Components:**
- Pages: PartnerOnboarding, Partners, Welcome
- Components: PartnerDashboard, GenerationBuilder, PerkCreationForm, AnalyticsTab
- Hooks: usePartnerOnboarding, usePartnerAnalytics, usePartnerGenerations

**Smart Contract Details:**
- **Ledger**: User balances, point accounting, stake tracking, partner TVL
- **Staking Manager**: Stake creation, yield calculation, position management
- **Oracle**: SUI/USD price feeds, TVL calculations, market data validation
- **Partner Contract**: TVL-backed capabilities, revenue sharing, quota enforcement

**Transaction Flow:**
- Transaction types: Stake SUI, claim rewards, redeem perks, partner operations
- Transaction adapter: @mysten/sui.js integration with gas optimization and error handling

## Key Features

1. **Decentralized Staking**: Users stake SUI tokens to earn Alpha Points
2. **Perk Marketplace**: Redeem points for various perks and benefits
3. **Partner System**: TVL-backed partner onboarding with revenue sharing
4. **Loan System**: Collateralized lending against staked positions
5. **Analytics**: Comprehensive tracking of user engagement and partner performance

## Technology Stack

- **Frontend**: React 18, TypeScript, Vite, Tailwind CSS
- **Blockchain**: Sui Move smart contracts
- **Wallet Integration**: @mysten/dapp-kit
- **State Management**: React Context API
- **Styling**: Tailwind CSS with custom components

## Implementation Notes

This architecture reflects the current implementation as of the documentation date, with deprecated components (Enoki zkLogin, SuiNS) removed for accuracy. All components listed are actively used in the production system.

## Usage

### Viewing the Diagrams

1. **For High-Level Overview**: Use `alpha-points-architecture.mermaid` for executive summaries
2. **For Technical Deep-Dive**: Use `alpha-points-system-architecture.mermaid` for comprehensive technical understanding
3. **For Implementation Details**: Reference `tech-stack.md` for exact versions and configurations

### Integration with Tools

These `.mermaid` files can be rendered in:
- **GitHub**: Automatically rendered in README files
- **Mermaid Live Editor**: https://mermaid-live-github-com.translate.goog/
- **VS Code**: Mermaid Preview extension
- **Documentation Tools**: GitBook, Notion, Confluence with Mermaid support

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