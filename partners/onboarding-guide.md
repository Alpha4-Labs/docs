# Partner Onboarding Guide

This comprehensive guide walks you through becoming an Alpha Points partner, from initial setup to your first revenue-generating perk.

## 🎯 Prerequisites

### **What You'll Need**
- **USDC Tokens**: Minimum $100 USDC for collateral (testnet or mainnet)
- **Sui Wallet**: Browser wallet or programmatic wallet for transactions
- **SUI Tokens**: For transaction gas fees (~$1-5 worth)
- **Development Environment**: Node.js, TypeScript/JavaScript (if building integrations)

### **Technical Knowledge**
- **Basic Blockchain**: Understanding of wallets, transactions, gas fees
- **Smart Contracts**: Familiarity with calling contract functions
- **API Integration**: REST APIs, webhooks (for advanced integrations)
- **USDC Handling**: ERC-20 token transfers and approvals

---

## 🚀 Step 1: Prepare Your USDC Collateral

### **Get USDC Tokens**

#### **For Testnet (Development)**
```bash
# Use Sui testnet faucet or testnet USDC
# Testnet USDC: 0xa1ec7fc00a6f40db9693ad1415d0c193ad3906494428cf252621037bd7117e29::usdc::USDC
```

#### **For Mainnet (Production)**
```bash
# Purchase USDC from exchanges (Coinbase, Binance, etc.)
# Bridge to Sui network via Wormhole or other bridges
# Mainnet USDC: 0xdba34672e30cb065b1f93e3ab55318768fd6fef66c15942c9f7cb846e2f900e7::usdc::USDC
```

### **Minimum Amounts**
- **Development**: $100 USDC minimum
- **DeFi Integration**: $1,000 USDC minimum
- **Enterprise Scale**: $10,000+ USDC recommended

---

## 🏗️ Step 2: Create Partner Vault

### **Using TypeScript SDK**

```typescript
import { SuiClient, getFullnodeUrl } from '@mysten/sui.js/client';
import { TransactionBlock } from '@mysten/sui.js/transactions';
import { Ed25519Keypair } from '@mysten/sui.js/keypairs/ed25519';

// Initialize client
const client = new SuiClient({ 
  url: getFullnodeUrl('testnet') // or 'mainnet'
});

// Your wallet keypair (secure this!)
const keypair = Ed25519Keypair.deriveKeypair('your-mnemonic-phrase');

// Contract addresses (update with deployed addresses)
const PACKAGE_ID = '0x...'; // Alpha Points package ID
const USDC_TYPE = '0xa1ec7fc00a6f40db9693ad1415d0c193ad3906494428cf252621037bd7117e29::usdc::USDC';

async function createPartnerVault() {
  const tx = new TransactionBlock();
  
  // Get your USDC coin object
  const usdcCoins = await client.getCoins({
    owner: keypair.toSuiAddress(),
    coinType: USDC_TYPE
  });
  
  const usdcCoin = usdcCoins.data[0]; // Use first USDC coin
  
  // Create partner vault with USDC collateral
  tx.moveCall({
    target: `${PACKAGE_ID}::partner_v3::create_partner_with_usdc_vault`,
    typeArguments: [USDC_TYPE],
    arguments: [
      tx.object(usdcCoin.coinObjectId), // USDC coin
      tx.pure('My Gaming Platform'), // Partner name
    ],
  });
  
  // Execute transaction
  const result = await client.signAndExecuteTransactionBlock({
    signer: keypair,
    transactionBlock: tx,
    options: {
      showEffects: true,
      showObjectChanges: true,
    },
  });
  
  console.log('Partner vault created:', result);
  return result;
}
```

### **Using Sui CLI**

```bash
# Create partner vault via CLI
sui client call \
  --package $PACKAGE_ID \
  --module partner_v3 \
  --function create_partner_with_usdc_vault \
  --type-args $USDC_TYPE \
  --args $USDC_COIN_ID "My Gaming Platform" \
  --gas-budget 10000000
```

### **What You Get**
- **PartnerCapV3**: Your partner capability object (keep secure!)
- **PartnerVault**: Your USDC collateral vault
- **Minting Rights**: Ability to mint points backed by your USDC

---

## 📝 Step 3: Register Integration

### **Register Your Platform**

```typescript
async function registerIntegration(
  partnerCapId: string,
  partnerVaultId: string
) {
  const tx = new TransactionBlock();
  
  // Get shared objects
  const registry = 'INTEGRATION_REGISTRY_ID'; // Shared registry object
  
  tx.moveCall({
    target: `${PACKAGE_ID}::generation_manager_v2::register_partner_integration`,
    arguments: [
      tx.object(registry), // IntegrationRegistry
      tx.object(partnerCapId), // Your PartnerCapV3
      tx.object(partnerVaultId), // Your PartnerVault
      tx.pure('My Gaming Platform'), // Integration name
      tx.pure('gaming'), // Integration type
      tx.pure(['https://api.mygame.com/webhooks/alpha-points']), // Optional webhook URL
      tx.object('0x6'), // Clock object
    ],
  });
  
  const result = await client.signAndExecuteTransactionBlock({
    signer: keypair,
    transactionBlock: tx,
  });
  
  console.log('Integration registered:', result);
  return result;
}
```

### **Integration Types**
- `gaming`: Games, achievements, tournaments
- `ecommerce`: Shopping, loyalty, referrals  
- `defi`: Yield farming, liquidity, governance
- `social`: Content creation, community engagement
- `api`: Generic API-based integration

---

## 🎮 Step 4: Define Your First Action

### **Example: Level Completion Action**

```typescript
async function registerLevelCompletionAction(
  registryId: string,
  integrationId: string,
  partnerCapId: string
) {
  const tx = new TransactionBlock();
  
  tx.moveCall({
    target: `${PACKAGE_ID}::generation_manager_v2::register_action`,
    arguments: [
      tx.object(registryId), // IntegrationRegistry
      tx.object(integrationId), // Your PartnerIntegration
      tx.object(partnerCapId), // Your PartnerCapV3
      tx.pure('level_completed'), // Action name (unique)
      tx.pure('Level Completed'), // Display name
      tx.pure('Player completed a game level'), // Description
      tx.pure('gaming'), // Category
      tx.pure(100), // Points per execution (100 points = $0.10)
      tx.pure([10]), // Max 10 executions per user per day
      tx.pure(5000), // 5 second cooldown between executions
      tx.pure(true), // Requires context data
      tx.pure([]), // No action-specific webhook
      tx.object('0x6'), // Clock object
    ],
  });
  
  const result = await client.signAndExecuteTransactionBlock({
    signer: keypair,
    transactionBlock: tx,
  });
  
  console.log('Level completion action registered:', result);
  return result;
}
```

### **Action Parameters Explained**
- **Points per execution**: How many points users earn (100 = $0.10 value)
- **Daily limits**: Prevent abuse (10 level completions per day max)
- **Cooldown**: Minimum time between executions (5 seconds)
- **Context data**: Additional info about the action (level number, score, etc.)

---

## ⚡ Step 5: Execute Actions (Mint Points)

### **When Users Complete Actions**

```typescript
async function executeAction(
  registryId: string,
  integrationId: string,
  actionId: string,
  partnerCapId: string,
  partnerVaultId: string,
  ledgerId: string,
  userAddress: string,
  levelNumber: number,
  score: number
) {
  const tx = new TransactionBlock();
  
  // Prepare context data
  const contextData = JSON.stringify({
    level: levelNumber,
    score: score,
    timestamp: Date.now(),
    action: 'level_completed'
  });
  
  tx.moveCall({
    target: `${PACKAGE_ID}::generation_manager_v2::execute_registered_action`,
    arguments: [
      tx.object(registryId), // IntegrationRegistry
      tx.object(integrationId), // PartnerIntegration
      tx.object(actionId), // RegisteredAction
      tx.object(partnerCapId), // PartnerCapV3
      tx.object(partnerVaultId), // PartnerVault
      tx.object(ledgerId), // LedgerV2
      tx.pure(userAddress), // User who completed action
      tx.pure(Array.from(Buffer.from(contextData))), // Context data as bytes
      tx.pure('game_client'), // Execution source
      tx.object('0x6'), // Clock object
    ],
  });
  
  const result = await client.signAndExecuteTransactionBlock({
    signer: keypair,
    transactionBlock: tx,
  });
  
  console.log(`Minted 100 points to user ${userAddress}:`, result);
  return result;
}
```

### **Integration Patterns**

#### **Backend Integration (Recommended)**
```typescript
// In your game server when player completes level
app.post('/level-completed', async (req, res) => {
  const { userId, levelNumber, score } = req.body;
  
  // Validate the completion server-side
  if (await validateLevelCompletion(userId, levelNumber, score)) {
    // Mint points via smart contract
    const result = await executeAction(
      REGISTRY_ID,
      INTEGRATION_ID,
      ACTION_ID,
      PARTNER_CAP_ID,
      PARTNER_VAULT_ID,
      LEDGER_ID,
      userId,
      levelNumber,
      score
    );
    
    res.json({ success: true, pointsEarned: 100, txHash: result.digest });
  } else {
    res.status(400).json({ error: 'Invalid level completion' });
  }
});
```

#### **Frontend Integration**
```typescript
// In your React app when user completes action
async function onLevelComplete(levelNumber: number, score: number) {
  try {
    const tx = buildExecuteActionTransaction(levelNumber, score);
    const result = await wallet.signAndExecuteTransactionBlock({ 
      transactionBlock: tx 
    });
    
    showNotification('🎉 You earned 100 Alpha Points!');
  } catch (error) {
    showNotification('❌ Failed to mint points');
  }
}
```

---

## 🎁 Step 6: Create Your First Perk

### **Example: Premium Skin Perk**

```typescript
async function createPremiumSkinPerk(
  partnerCapId: string,
  partnerVaultId: string
) {
  const tx = new TransactionBlock();
  
  // Perk metadata
  const metadata = JSON.stringify({
    type: 'digital_item',
    category: 'skin',
    rarity: 'legendary',
    image: 'https://mygame.com/assets/legendary-sword.png',
    description: 'Exclusive legendary sword skin with particle effects'
  });
  
  tx.moveCall({
    target: `${PACKAGE_ID}::perk_manager_v2::create_perk_definition`,
    typeArguments: [USDC_TYPE],
    arguments: [
      tx.object(partnerCapId), // PartnerCapV3
      tx.object(partnerVaultId), // PartnerVault
      tx.pure('Legendary Sword Skin'), // Perk name
      tx.pure('Exclusive legendary sword with particle effects'), // Description
      tx.pure(5000), // Points cost (5000 points = $5 value)
      tx.pure(3500000), // USDC cost (3.5 USDC = $3.50, profit margin)
      tx.pure([100]), // Max 100 redemptions
      tx.pure(Array.from(Buffer.from(metadata))), // Metadata
      tx.object('0x6'), // Clock object
    ],
  });
  
  const result = await client.signAndExecuteTransactionBlock({
    signer: keypair,
    transactionBlock: tx,
  });
  
  console.log('Premium skin perk created:', result);
  return result;
}
```

### **Perk Economics**
- **Points Cost**: 5000 points = $5.00 user value
- **USDC Price**: $3.50 user payment
- **Your Revenue**: $2.45 (70% of $3.50)
- **Platform Fee**: $0.70 (20% of $3.50)
- **Vault Growth**: $0.35 (10% of $3.50, increases your backing capacity)

---

## 💰 Step 7: Revenue Flow

### **When Users Redeem Your Perk**

The redemption happens automatically when users have enough points and USDC:

```typescript
// User calls this function to redeem your perk
async function redeemPerk(
  perkId: string,
  ledgerId: string,
  partnerVaultId: string,
  userUsdcCoinId: string,
  userAddress: string
) {
  const tx = new TransactionBlock();
  
  tx.moveCall({
    target: `${PACKAGE_ID}::perk_manager_v2::redeem_perk`,
    typeArguments: [USDC_TYPE],
    arguments: [
      tx.object(perkId), // Your PerkDefinition
      tx.object(ledgerId), // LedgerV2
      tx.object(partnerVaultId), // Your PartnerVault
      tx.object(userUsdcCoinId), // User's USDC payment
      tx.pure(userAddress), // User address
      tx.object('0x6'), // Clock object
    ],
  });
  
  // User signs and executes this transaction
  // You automatically receive 70% of the USDC payment
  // 10% goes to your vault (increases backing capacity)
  // 20% goes to platform treasury
}
```

### **Revenue Tracking**

```typescript
// Check your earnings
async function checkPartnerEarnings(partnerVaultId: string) {
  const vaultObject = await client.getObject({
    id: partnerVaultId,
    options: { showContent: true }
  });
  
  // Parse vault data to see:
  // - Total USDC balance
  // - Points minted
  // - Available for withdrawal
  console.log('Vault info:', vaultObject);
}
```

---

## 📊 Step 8: Monitor & Optimize

### **Key Metrics to Track**

1. **Points Minted**: How many points your actions generate
2. **Perk Redemptions**: Revenue from your perks
3. **User Engagement**: Unique users completing actions
4. **Conversion Rate**: Actions → Perk redemptions
5. **Revenue per User**: Average USDC earned per user

### **Optimization Strategies**

#### **Action Optimization**
```typescript
// Add more diverse actions
await registerAction('achievement_unlocked', 50); // Smaller reward, more frequent
await registerAction('daily_login', 25); // Retention incentive
await registerAction('friend_referral', 500); // Growth incentive
```

#### **Perk Strategy**
```typescript
// Create tiered perks
await createPerk('Common Skin', 1000, 0.70); // Low cost, high volume
await createPerk('Rare Weapon', 2500, 1.75); // Mid-tier
await createPerk('Legendary Set', 10000, 7.00); // Premium offering
```

### **Analytics Integration**

```typescript
// Track custom events
function trackPointsEarned(userId: string, action: string, points: number) {
  analytics.track('Points Earned', {
    userId,
    action,
    points,
    timestamp: Date.now(),
    source: 'alpha_points'
  });
}

function trackPerkRedeemed(userId: string, perkName: string, usdcSpent: number) {
  analytics.track('Perk Redeemed', {
    userId,
    perkName,
    usdcSpent,
    pointsSpent: usdcSpent * 1000, // Assuming 1:1000 ratio
    timestamp: Date.now()
  });
}
```

---

## 🔧 Advanced Features

### **Webhook Implementation**

```typescript
// Set up webhook endpoint to receive real-time notifications
app.post('/webhooks/alpha-points', (req, res) => {
  const { event, data } = req.body;
  
  switch (event) {
    case 'action_executed':
      console.log(`User ${data.userAddress} earned ${data.pointsEarned} points`);
      // Update your database, send notifications, etc.
      break;
      
    case 'perk_redeemed':
      console.log(`Perk ${data.perkName} redeemed for ${data.usdcAmount} USDC`);
      // Fulfill the perk (deliver digital item, etc.)
      break;
      
    case 'vault_low_balance':
      console.log('Warning: Vault balance getting low, add more USDC');
      // Alert your team to add more collateral
      break;
  }
  
  res.status(200).send('OK');
});
```

### **DeFi Integration**

Once you have $1,000+ USDC in your vault:

```typescript
// Transfer vault to yield protocol (e.g., Scallop)
async function integrateWithDefi(partnerVaultId: string, protocolAddress: string) {
  const tx = new TransactionBlock();
  
  tx.moveCall({
    target: `${PACKAGE_ID}::partner_v3::transfer_vault_to_defi`,
    arguments: [
      tx.object(partnerVaultId),
      tx.pure(protocolAddress),
      tx.pure('scallop'), // Protocol name
    ],
  });
  
  // Your USDC earns yield while still backing points
  // You get 50% of yield, protocol gets 50%
}
```

---

## ✅ Checklist

### **Pre-Launch**
- [ ] USDC collateral deposited (minimum $100)
- [ ] Partner vault and capability created
- [ ] Integration registered with webhook URL
- [ ] At least one action defined and tested
- [ ] First perk created with attractive pricing
- [ ] Backend integration implemented and tested
- [ ] Analytics tracking set up

### **Launch**
- [ ] Actions generating points for users
- [ ] Perks available for redemption
- [ ] Revenue flowing to your address
- [ ] Webhooks receiving notifications
- [ ] User feedback collection started

### **Post-Launch**
- [ ] Performance metrics monitored daily
- [ ] Additional actions added based on user behavior
- [ ] Perk catalog expanded with different price points
- [ ] DeFi integration considered (if vault > $1,000)
- [ ] User retention strategies implemented

---

## 🆘 Troubleshooting

### **Common Issues**

#### **"Insufficient backing" error**
```typescript
// Check vault balance vs points minted
const vaultInfo = await getVaultInfo(partnerVaultId);
if (vaultInfo.reservedBacking >= vaultInfo.totalBalance * 0.9) {
  console.log('Add more USDC collateral to continue minting points');
}
```

#### **"Rate limit exceeded" error**
```typescript
// Check action execution limits
const actionInfo = await getActionInfo(actionId);
if (actionInfo.dailyExecutions >= actionInfo.maxDailyExecutions) {
  console.log('Daily execution limit reached, wait until tomorrow');
}
```

#### **Gas estimation failures**
```typescript
// Use higher gas budget for complex transactions
const tx = new TransactionBlock();
tx.setGasBudget(20000000); // 0.02 SUI
```

### **Support Resources**
- **Discord**: Real-time developer support
- **GitHub Issues**: Bug reports and feature requests  
- **Documentation**: [Complete API reference](../modules/api-reference.md)
- **Partner Portal**: Enterprise support for large integrations

---

## 🎯 Next Steps

Congratulations! You're now a fully onboarded Alpha Points partner. Here's what to focus on next:

1. **Launch & Monitor**: Deploy your integration and track initial metrics
2. **Iterate**: Add more actions and perks based on user feedback
3. **Scale**: Consider DeFi integration once your vault reaches $1,000+
4. **Community**: Join our partner Discord for best practices and support

**Happy building!** 🚀
