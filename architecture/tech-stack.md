# Alpha Points Tech Stack

## **1. BLOCKCHAIN INFRASTRUCTURE**

### **Move Language & Sui Blockchain**
- **Move Language**: `2024.alpha` edition
- **Sui Blockchain**: Layer 1 blockchain platform
- **Package Version**: `0.1.1`
- **Published Address**: `0xbae3eef628211af44c386e621142118bdee8825b059e0514bf3729638109cd3a`

### **Smart Contract Dependencies**
- **Core Framework**: Sui Framework (`0x2`)
- **Standard Library**: Move Standard Library
- **Package Address**: `alpha_points = "0x0"`

---

## **2. FRONTEND APPLICATIONS**

### **Core Framework**
- **React**: `^18.2.0` - Component-based UI library
- **TypeScript**: `^5.2.2` - Type-safe JavaScript
- **Vite**: `^5.4.19` - Fast build tool and development server

### **Build & Development Tools**
- **@vitejs/plugin-react**: `^4.2.1` - React plugin for Vite
- **PostCSS**: `^8.4.32` - CSS transformations
- **Autoprefixer**: `^10.4.16` - CSS vendor prefixes
- **ESLint**: Modern linting configuration

### **TypeScript Configuration**
- **Target**: `ES2020`
- **Module System**: `ESNext` with bundler resolution
- **JSX**: `react-jsx` transformation
- **Strict Mode**: Enabled with comprehensive type checking
- **Libraries**: `["ES2020", "DOM", "DOM.Iterable"]`

---

## **3. BLOCKCHAIN INTEGRATION**

### **Sui SDK Integration**
- **@mysten/sui**: `^1.30.1` - Core Sui JavaScript SDK
- **@mysten/dapp-kit**: `^0.16.5` - Wallet connection and dApp utilities
- **@mysten/suins**: `^0.7.17` - SuiNS integration (legacy)

### **Wallet & Transaction Management**
- **@tanstack/react-query**: `^5.17.9` - Server state management
- **Wallet Connection**: `@mysten/dapp-kit` hooks
  - `useCurrentAccount` - Account state management
  - `useConnectWallet` - Wallet connection flow
  - `useSignAndExecuteTransaction` - Transaction signing

### **Transaction Flow**
- **Gas Optimization**: Automated gas estimation
- **Error Handling**: Comprehensive retry logic
- **State Management**: React Query for transaction state
- **RPC Proxy**: Development CORS handling via Vite proxy

---

## **4. USER INTERFACE & STYLING**

### **CSS Framework**
- **Tailwind CSS**: `^3.4.0` - Utility-first CSS framework
- **tailwind-merge**: `^3.3.0` - Intelligent class merging
- **PostCSS**: `^8.4.32` - CSS processing pipeline

### **UI Components**
- **@headlessui/react**: `^2.2.2` - Unstyled, accessible components
- **@heroicons/react**: `^2.2.0` - SVG icon library
- **class-variance-authority**: `^0.7.1` - Component variant management
- **clsx**: `^2.1.1` - Conditional class name utility

### **Interactive Elements**
- **Swiper**: `^11.2.6` - Modern touch slider
- **Framer Motion**: `^11.15.0` (rewards app) - Animation library
- **Lucide React**: `^0.513.0` (rewards app) - Icon library

---

## **5. ROUTING & NAVIGATION**

### **Client-Side Routing**
- **react-router-dom**: `^7.6.2` - Declarative routing
- **Protected Routes**: Authentication-based route protection
- **Multi-App Architecture**: Separate routing for frontend/partners

### **Application Ports**
- **Frontend**: Port 5173 (main user app)
- **Partners**: Port 5174 (partner dashboard)
- **Rewards**: Development port for white-label marketplace

---

## **6. STATE MANAGEMENT**

### **React State Management**
- **React Context API**: Global state management
- **useReducer**: Complex state logic
- **@tanstack/react-query**: Server state and caching
- **Zustand**: `^5.0.5` (rewards app) - Lightweight state management
- **Immer**: `^10.1.1` (rewards app) - Immutable state updates

### **Custom Hooks**
- **useAlphaPoints**: Point balance and transactions
- **useStakePositions**: Staking position management
- **usePerkData**: Perk marketplace data
- **useLoans**: Loan system integration
- **usePartnerAnalytics**: Partner dashboard metrics

---

## **7. DATA VISUALIZATION**

### **Charts & Analytics**
- **Recharts**: `^2.15.3` - React charting library
- **Custom Visualizations**: TVL tracking, performance metrics
- **Real-time Updates**: Live data integration with blockchain events

---

## **8. NOTIFICATIONS & FEEDBACK**

### **User Notifications**
- **react-toastify**: `^9.1.3` - Toast notifications
- **react-hot-toast**: `^2.4.1` - Alternative toast system
- **Custom Error Handling**: Comprehensive error boundaries

---

## **9. SECURITY & AUTHENTICATION**

### **Cryptographic Functions**
- **crypto-js**: `^4.2.0` - Cryptographic utilities
- **JWT Processing**: `jwt-decode ^4.0.0` - JWT token handling
- **@types/crypto-js**: `^4.2.2` - TypeScript definitions

### **Wallet Security**
- **Non-custodial**: User controls private keys
- **Multi-wallet Support**: Support for various Sui wallets
- **Transaction Signing**: Secure transaction approval flow

---

## **10. SDK & INTEGRATION**

### **Alpha4 Onboard SDK**
- **Version**: `1.0.0`
- **Language**: Pure JavaScript (ES5+ compatible)
- **Bundle Size**: Minified and optimized
- **CDN Distribution**: `https://onboard.alpha4.io/sui-points-adapter.js`

### **SDK Features**
- **Zero-Dev Integration**: Drop-in JavaScript solution
- **Automatic Event Detection**: Form submissions, button clicks, page changes
- **Rate Limiting**: Built-in spam prevention
- **Wallet Integration**: Auto-detection of Sui wallets
- **Event Queue**: Offline event storage and processing

### **Build Tools**
- **Terser**: `^5.16.0` - JavaScript minification
- **Source Maps**: Development debugging support
- **HTTP Server**: `^14.1.1` - Local development server

---

## **11. DEVELOPMENT ENVIRONMENT**

### **Development Servers**
- **Vite Dev Server**: Hot module replacement
- **Proxy Configuration**: SUI RPC proxy for CORS handling
- **Port Configuration**: 
  - Frontend: 3000 (configured in vite.config.ts)
  - Partners: Host mode enabled
  - Auto-open browser on start

### **Code Quality**
- **ESLint**: Modern configuration with React rules
- **TypeScript**: Strict type checking
- **Prettier**: Code formatting (implied by consistent formatting)
- **Git Hooks**: Pre-commit validation

### **Testing Strategy**
- **Unit Testing**: Component and utility testing
- **Integration Testing**: Smart contract interaction testing
- **E2E Testing**: Full user flow validation

---

## **12. EXTERNAL INTEGRATIONS**

### **Discord Integration**
- **@discord/embedded-app-sdk**: `^2.0.0` - Discord app embedding
- **Bot Integration**: Automated Discord interactions
- **Activity Tracking**: Discord-based engagement metrics

### **Third-Party APIs**
- **Price Oracles**: Real-time SUI/USD price feeds
- **RPC Endpoints**: Sui blockchain connectivity
- **External Services**: Partner API integrations

---

## **13. PERFORMANCE & OPTIMIZATION**

### **Bundle Optimization**
- **Code Splitting**: Route-based code splitting
- **Tree Shaking**: Unused code elimination
- **Asset Optimization**: Image and resource compression
- **CDN Distribution**: Global content delivery

### **Caching Strategy**
- **React Query**: Intelligent server state caching
- **Browser Storage**: Local state persistence
- **RPC Caching**: Blockchain query optimization

---

## **14. DEPLOYMENT & INFRASTRUCTURE**

### **Build Process**
- **TypeScript Compilation**: `tsc -b && vite build`
- **Asset Bundling**: Vite production builds
- **Environment Variables**: Template-based configuration
- **Multi-environment Support**: Development, staging, production

### **Distribution**
- **Static Site Generation**: Optimized for CDN deployment
- **Asset Optimization**: Minification and compression
- **Source Maps**: Production debugging support

---

## **15. MONITORING & ANALYTICS**

### **Error Tracking**
- **Custom Error Boundaries**: React error handling
- **Transaction Monitoring**: Blockchain transaction tracking
- **Performance Metrics**: User interaction analytics

### **Debug Tools**
- **Development Mode**: Enhanced logging and debugging
- **Console Logging**: Structured debug output
- **Network Monitoring**: RPC call tracking

---

## **16. VERSION MANAGEMENT**

### **Package Versioning**
- **Frontend**: `0.1.0`
- **Partners**: `0.1.0`
- **Rewards**: `1.0.0`
- **SDK**: `1.0.0`
- **Smart Contracts**: `0.1.1`

### **Dependency Management**
- **npm**: Package manager
- **Lock Files**: Reproducible builds
- **Peer Dependencies**: Flexible integration
- **Override Configuration**: Version conflict resolution

---

## **ARCHITECTURE SUMMARY**

The Alpha Points protocol utilizes a modern, type-safe tech stack optimized for blockchain integration and user experience. The architecture emphasizes:

- **Developer Experience**: TypeScript, modern tooling, hot reloading
- **Performance**: Vite builds, React Query caching, code splitting
- **Security**: Non-custodial wallets, cryptographic functions, rate limiting
- **Scalability**: Modular architecture, CDN distribution, efficient bundling
- **Maintainability**: Strong typing, consistent patterns, comprehensive testing

All components are production-ready with comprehensive error handling, performance optimization, and security best practices. 