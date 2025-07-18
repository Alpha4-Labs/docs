# Alpha Points Technology Stack

## Core Stack
- **Frontend**: React 18.2.0, TypeScript 5.2.2, Vite 5.4.19, Tailwind CSS, @mysten/sui v1.30.1, @mysten/dapp-kit v0.16.5.
- **Smart Contracts**: Sui Move 2024.alpha.
- **Build/Deploy**: Vite for bundling, Sui CLI for contract deployment.

## External Dependencies and Integrations

| Dependency | Version/Purpose | Integration Points | Risks/Mitigations |
|------------|-----------------|---------------------|-------------------|
| **RateOracle/ExternalPriceOracle** | Custom (oracle.move) | Price fetching in partner.move (e.g., price_in_usdc for collateral valuation); used in hooks like useTVLCalculation.ts. | Staleness (E_PRICE_TOO_STALE); Mitigate with assert_not_stale and max staleness checks (MAX_PRICE_STALENESS_EPOCHS=24). |
| **TradePort API** | GraphQL API | NFT floor price updates in partner.move (update_floor_price); queried via BlockVision or direct in eventQuerying.ts. | API downtime; Use retry.ts and fallback to cached prices. |
| **Kiosk** | Sui Kiosk module | NFT collateral locking in create_partner_cap_with_nft_bundle (partner.move); verified in frontend via usePerkData.ts. | Ownership mismatches (E_KIOSK_NOT_OWNED); Validate with kiosk_owner_cap before calls. |
| **BlockVision API** | Latest | Event querying in eventQuerying.ts (e.g., PartnerCap events); configured via VITE_BLOCKVISION_API_KEY. | Rate limits; Implement globalRateLimiter.ts and fallback to Sui RPC. |
| **Discord API** | v10 | Integration in discord.ts (rewards/); used for auth/events in frontend/components/DiscordHandleModal.tsx. | Token expiration; Handle via refresh tokens and errorCodes.ts. |
| **Walrus Storage** | Latest | Blob storage for assets (e.g., in utils/cache.ts); referenced in partner.move for NFT metadata. | Availability; Use redundancy with local caching. |
| **Enoki SDK** | Latest | Sponsored TXs (VITE_ENOKI_KEY); used in transaction.ts for gasless flows. | Sponsorship limits; Monitor via VITE_DEFAULT_SPONSORED_GAS_BUDGET and quotaValidator. |

## Risks and Best Practices
- **Oracle Dependencies**: Always check staleness; fallback to admin overrides.
- **API Keys**: Store securely in .env; rotate regularly.
- **Versions**: Pin dependencies in package.json; test upgrades for breaking changes (e.g., Sui SDK). 