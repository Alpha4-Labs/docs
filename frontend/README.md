# Alpha Points Frontend Documentation

## Overview
The frontend is a React 18.2.0 application built with TypeScript 5.2.2 and Vite 5.4.19 for fast development and bundling. It uses Tailwind CSS for styling and @mysten/sui v1.30.1 for Sui blockchain interactions. The app runs on port 5173 (dev) and integrates with smart contracts in `../sources/` (e.g., via hooks calling Move functions like `create_partner_cap_with_collateral` in `partner.move`).

Key directories:
- `src/components/`: UI elements (e.g., PartnerDashboard/, LoanPanel.tsx).
- `src/hooks/`: Custom React hooks for data fetching (e.g., useAlphaPoints.ts fetches points via Sui queries).
- `src/utils/`: Helpers (e.g., transaction.ts for tx building, eventQuerying.ts for event handling).
- `src/pages/`: Main routes (e.g., DashboardPage.tsx).

For setup, see [Environment Variables](#environment-variables) and the original README in `../frontend/README.md`.

## Component Catalog

| Component/Hook | Description | Key Props/Params | Source | Smart Contract Integration |
|---------------|-------------|------------------|--------|----------------------------|
| `PartnerDashboard.tsx` | Main dashboard for partners; handles analytics, generations, perks. | `partnerId: string` (optional) | `src/components/PartnerDashboard/` | Calls `usePartnerAnalytics.ts` hook, which queries `partner.move` for quotas/events. |
| `LoanPanel.tsx` | UI for loan management; displays positions and actions. | `loanData: LoanData` | `src/components/LoanPanel.tsx` | Uses `useLoans.ts` to interact with `loan.move` (e.g., borrow/repay functions). |
| `useAlphaPoints.ts` | Hook to fetch and manage user's Alpha Points balance. | None (uses context) | `src/hooks/useAlphaPoints.ts` | Queries ledger.move for balances; updates on events from integration.move. |
| `usePartnerQuotas.ts` | Hook for partner quota monitoring. | `partnerCapId: ID` | `src/hooks/usePartnerQuotas.ts` | Reads daily_quota_pts from partner.move; handles resets via clock. |
| `transaction.ts` | Utility for building/signing Sui transactions. | `txData: TxData` | `src/utils/transaction.ts` | Builds PTBs for calling Move functions (e.g., stake in staking_manager.move). |

For full list, see source code. Add JSDoc to files for detailed props.

## Environment Variables
See `../frontend/env.template` for required vars like `VITE_SUI_RPC_URL` (Sui RPC endpoint) and `VITE_CONTRACT_PACKAGE_ID` (deployed package ID from Move.toml).

## Updates and Guides
- [Updates](./updates/) : Consolidated change logs.
  - [ZKLOGIN_DEPRECATION.md](./updates/ZKLOGIN_DEPRECATION.md): zkLogin migration notes.
  - [TRANSACTION_REFRESH_SYSTEM.md](./updates/TRANSACTION_REFRESH_SYSTEM.md): TX refresh system updates.
  - [PARTNER_DASHBOARD_STAT_FIXES.md](./updates/PARTNER_DASHBOARD_STAT_FIXES.md): Partner stats fixes.
  - [SWIPER_INTEGRATION_UPDATE.md](./updates/SWIPER_INTEGRATION_UPDATE.md): Swiper integration changes.
  - [LOANS_INTEGRATION_UPDATE.md](./updates/LOANS_INTEGRATION_UPDATE.md): Loans feature updates.
  - [FLOATING_ARROW_NAVIGATION.md](./updates/FLOATING_ARROW_NAVIGATION.md): Navigation UI updates.
  - [ALPHA_POINTS_CONVERSION_UPDATE.md](./updates/ALPHA_POINTS_CONVERSION_UPDATE.md): Points conversion changes.
  - [DASHBOARD_GENERATION_RESTRUCTURE.md](./updates/DASHBOARD_GENERATION_RESTRUCTURE.md): Dashboard restructure.
  - [SPONSORSHIP_SETUP.md](./updates/SPONSORSHIP_SETUP.md): Sponsorship configuration.
- [User Guides](./user-guides/) : Feature walkthroughs (e.g., perk creation). 