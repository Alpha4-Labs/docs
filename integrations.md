# Alpha Points Integrations Guide

This document covers how the frontend (React/TypeScript app) integrates with Sui Move smart contracts in `sources/`. Focus is on key flows, hooks/utils, and best practices. For architecture, see [architecture/](./architecture/).

## Overview
The frontend uses @mysten/sui SDK for queries/transactions. Hooks (e.g., useAlphaPoints.ts) fetch data via Sui RPC (configured in env.template). Utils (e.g., transaction.ts) build Programmable Transaction Blocks (PTBs) to call Move functions. Events from contracts (e.g., PartnerCapCreated) are handled via eventQuerying.ts.

## Key Integration Points

| Frontend Element | Smart Contract | Description | Example Flow |
|------------------|----------------|-------------|-------------|
| `useAlphaPoints.ts` (hook) | ledger.move | Fetches user's Alpha Points balance. | 1. Hook calls Sui getObject (VITE_LEDGER_ID). 2. Parses balance from shared object. 3. Updates on events like mint from integration.move. |
| `useLoans.ts` (hook) | loan.move | Manages loan positions (borrow/repay). | 1. Query loan objects via Sui RPC. 2. Build PTB in transaction.ts to call borrow(). 3. Sign/execute TX; handle success via useTransactionSuccess.ts. |
| `usePartnerQuotas.ts` (hook) | partner.move | Monitors partner quotas/daily resets. | 1. Fetch PartnerCap object. 2. Check daily_quota_pts and mint_remaining_today. 3. Reset on epoch change (via clock). |
| `transaction.ts` (util) | Various (e.g., staking_manager.move) | Builds/signs TXs for actions like staking. | ```typescript
import { TransactionBlock } from '@mysten/sui.js';
// Example: Stake SUI in staking_manager.move
export async function buildStakeTx(amount: number) {
  const txb = new TransactionBlock();
  txb.moveCall({
    target: `${VITE_PACKAGE_ID}::staking_manager::stake`,
    arguments: [txb.pure(amount)],
  });
  return txb;
}
``` Integrates with env vars like VITE_PACKAGE_ID. |
| `eventQuerying.ts` (util) | All event-emitting modules (e.g., partner.move) | Queries events via BlockVision API. | Uses VITE_BLOCKVISION_API_KEY to fetch events like PartnerCapCreatedWithCollateral.

## Best Practices
- **Error Handling**: Use retry.ts for RPC failures; check errorCodes.ts for Move errors (e.g., E_COLLATERAL_VALUE_ZERO).
- **Security**: Enable privacy.ts for hashing; use sponsored TXs (Enoki via VITE_ENOKI_KEY) for user-friendly flows.
- **Testing**: Mock Sui SDK in tests; use testnet (VITE_SUI_NETWORK=testnet) for dev.
- **Deprecations**: See ZKLOGIN_DEPRECATION.md for auth changes.

## Deployment and Testing Workflows

### Deployment
- **Frontend**: Run `yarn build` (Vite outputs to dist/). Deploy to Vercel/Netlify with env vars from .env (e.g., VITE_SUI_NETWORK=mainnet). Use CI/CD for auto-deploys on push to main.
- **Smart Contracts**: Use Sui CLI: `sui move build` then `sui client publish` (updates Move.toml with new package ID). Set VITE_PACKAGE_ID in env.
- **SDK**: Deploy to CDN (e.g., Cloudflare) via wrangler.toml; minify with Terser.

### Testing
- **Unit**: Jest for components/hooks (e.g., test useAlphaPoints.ts with mocked Sui SDK).
- **Integration**: Mock RPC in tests; use testnet for E2E with Cypress.
- **Workflow**: Run `yarn test`; monitor TXs via Sui explorer. Test edge cases like E_PRICE_TOO_STALE.

For deployment/testing, see [deployment.md](#) (TODO). Report issues in GitHub. 