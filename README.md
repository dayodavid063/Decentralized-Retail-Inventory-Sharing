# Decentralized Retail Inventory Sharing

A blockchain-based platform that enables retailers to share inventory across their networks, optimizing stock distribution and reducing waste through decentralized smart contracts.

## Overview

The Decentralized Retail Inventory Sharing system allows verified retailers to register their available inventory, establish sharing agreements with other merchants, and automatically handle fulfillment and settlement processes. This creates a collaborative ecosystem where retailers can access each other's inventory to meet customer demand while maintaining trust and transparency.

## Architecture

The platform consists of five interconnected smart contracts that work together to manage the entire inventory sharing lifecycle:

### 1. Retailer Verification Contract
**Purpose**: Validates and manages legitimate merchant participants

**Key Features**:
- Merchant identity verification and KYC compliance
- Business license validation
- Reputation scoring and history tracking
- Blacklist management for fraudulent actors
- Multi-tier verification levels (basic, premium, enterprise)

**Functions**:
- `verifyRetailer(address merchant, bytes32 businessId)`: Verify a new retailer
- `updateVerificationStatus(address merchant, uint8 status)`: Update verification level
- `getRetailerInfo(address merchant)`: Retrieve retailer verification details
- `isVerifiedRetailer(address merchant)`: Check if retailer is verified

### 2. Inventory Registration Contract
**Purpose**: Records and manages available merchandise across the network

**Key Features**:
- Product catalog management with standardized SKU system
- Real-time inventory tracking and updates
- Location-based inventory mapping
- Category and attribute filtering
- Automated stock level notifications

**Functions**:
- `registerInventory(string sku, uint256 quantity, uint256 price)`: Add inventory
- `updateStock(string sku, uint256 newQuantity)`: Update stock levels
- `getAvailableInventory(string category)`: Query available products
- `reserveInventory(string sku, uint256 quantity)`: Reserve items for orders

### 3. Sharing Agreement Contract
**Purpose**: Manages terms and conditions for inventory access between retailers

**Key Features**:
- Flexible agreement templates (revenue share, fixed fee, commission-based)
- Geographic boundary definitions
- Product category restrictions
- Duration and renewal terms
- Automatic agreement enforcement

**Functions**:
- `createSharingAgreement(address partner, AgreementTerms terms)`: Establish new agreement
- `acceptAgreement(bytes32 agreementId)`: Accept proposed terms
- `modifyAgreement(bytes32 agreementId, AgreementTerms newTerms)`: Update existing agreement
- `terminateAgreement(bytes32 agreementId)`: End sharing relationship

### 4. Fulfillment Tracking Contract
**Purpose**: Records and monitors order processing across the network

**Key Features**:
- End-to-end order lifecycle tracking
- Multi-party fulfillment coordination
- Shipping and delivery status updates
- Quality assurance and dispute handling
- Performance metrics and analytics

**Functions**:
- `createOrder(string sku, uint256 quantity, address customer)`: Initiate new order
- `updateOrderStatus(bytes32 orderId, OrderStatus status)`: Update order progress
- `confirmDelivery(bytes32 orderId, bytes32 deliveryProof)`: Confirm successful delivery
- `reportIssue(bytes32 orderId, string issueDescription)`: Report fulfillment problems

### 5. Settlement Contract
**Purpose**: Handles automated compensation for shared inventory transactions

**Key Features**:
- Multi-currency support (ETH, stablecoins, tokens)
- Automated payment calculations based on agreement terms
- Escrow functionality for dispute resolution
- Gas optimization for batch settlements
- Detailed transaction history and reporting

**Functions**:
- `processSettlement(bytes32 orderId)`: Calculate and execute payment
- `initiateDispute(bytes32 orderId, string reason)`: Start dispute process
- `releaseEscrow(bytes32 orderId, address recipient)`: Release held funds
- `batchSettle(bytes32[] orderIds)`: Process multiple settlements

## Getting Started

### Prerequisites
- Node.js 16+
- Hardhat development environment
- Web3 wallet (MetaMask recommended)
- Sufficient ETH for gas fees

### Installation

```bash
# Clone the repository
git clone https://github.com/your-org/decentralized-retail-inventory
cd decentralized-retail-inventory

# Install dependencies
npm install

# Compile smart contracts
npx hardhat compile

# Run tests
npx hardhat test

# Deploy to local network
npx hardhat node
npx hardhat run scripts/deploy.js --network localhost
```

### Configuration

1. **Environment Setup**
   ```bash
   cp .env.example .env
   # Configure your private keys and network settings
   ```

2. **Network Configuration**
   Update `hardhat.config.js` with your preferred networks:
   ```javascript
   networks: {
     mainnet: { url: "YOUR_RPC_URL", accounts: ["PRIVATE_KEY"] },
     polygon: { url: "YOUR_POLYGON_RPC", accounts: ["PRIVATE_KEY"] },
     testnet: { url: "YOUR_TESTNET_RPC", accounts: ["PRIVATE_KEY"] }
   }
   ```

3. **Contract Deployment**
   ```bash
   # Deploy to testnet
   npx hardhat run scripts/deploy.js --network testnet
   
   # Verify contracts
   npx hardhat verify CONTRACT_ADDRESS --network testnet
   ```

## Usage Examples

### For Retailers

**Register as a Verified Retailer**:
```javascript
const verificationContract = await ethers.getContractAt("RetailerVerification", CONTRACT_ADDRESS);
await verificationContract.verifyRetailer(merchantAddress, businessId);
```

**Add Inventory**:
```javascript
const inventoryContract = await ethers.getContractAt("InventoryRegistration", CONTRACT_ADDRESS);
await inventoryContract.registerInventory("SKU-12345", 100, ethers.utils.parseEther("29.99"));
```

**Create Sharing Agreement**:
```javascript
const sharingContract = await ethers.getContractAt("SharingAgreement", CONTRACT_ADDRESS);
const terms = {
  revenueSharePercent: 15,
  geographicRadius: 50, // miles
  categories: ["electronics", "clothing"],
  duration: 365 // days
};
await sharingContract.createSharingAgreement(partnerAddress, terms);
```

### For Customers

**Browse Available Inventory**:
```javascript
const inventoryContract = await ethers.getContractAt("InventoryRegistration", CONTRACT_ADDRESS);
const availableItems = await inventoryContract.getAvailableInventory("electronics");
```

**Place Order**:
```javascript
const fulfillmentContract = await ethers.getContractAt("FulfillmentTracking", CONTRACT_ADDRESS);
await fulfillmentContract.createOrder("SKU-12345", 2, customerAddress, {
  value: ethers.utils.parseEther("59.98")
});
```

## API Documentation

### Smart Contract Events

**RetailerVerification Events**:
- `RetailerVerified(address indexed merchant, uint8 verificationLevel)`
- `VerificationRevoked(address indexed merchant, string reason)`

**InventoryRegistration Events**:
- `InventoryAdded(address indexed retailer, string sku, uint256 quantity)`
- `StockUpdated(string indexed sku, uint256 newQuantity)`
- `InventoryReserved(string indexed sku, uint256 quantity, address customer)`

**Order Events**:
- `OrderCreated(bytes32 indexed orderId, address customer, string sku)`
- `OrderStatusChanged(bytes32 indexed orderId, OrderStatus newStatus)`
- `DeliveryConfirmed(bytes32 indexed orderId, address customer)`

### Error Codes

- `RV001`: Retailer not verified
- `IR002`: Insufficient inventory
- `SA003`: Invalid sharing agreement
- `FT004`: Order not found
- `SC005`: Settlement failed

## Security Considerations

- All contracts undergo comprehensive security audits
- Multi-signature wallet integration for administrative functions
- Rate limiting to prevent spam and DoS attacks
- Encrypted sensitive data storage
- Regular security updates and patches

## Gas Optimization

The platform implements several gas optimization strategies:
- Batch processing for multiple operations
- Efficient data structures and storage patterns
- Layer 2 scaling solutions compatibility
- Optional meta-transaction support for gasless user experience

## Contributing

We welcome contributions from the community. Please read our contributing guidelines and submit pull requests for review.

### Development Workflow
1. Fork the repository
2. Create a feature branch
3. Write tests for new functionality
4. Ensure all tests pass
5. Submit a pull request with detailed description

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

For technical support and questions:
- GitHub Issues: [Repository Issues](https://github.com/your-org/decentralized-retail-inventory/issues)
- Documentation: [Full Documentation](https://docs.inventory-sharing.com)
- Community Discord: [Join our Discord](https://discord.gg/inventory-sharing)

## Roadmap

**Phase 1** (Q2 2025): Core contract deployment and basic functionality
**Phase 2** (Q3 2025): Advanced analytics and reporting features
**Phase 3** (Q4 2025): Cross-chain compatibility and Layer 2 integration
**Phase 4** (Q1 2026): AI-powered inventory optimization and demand forecasting

---

*Built with ❤️ for the decentralized retail ecosystem*
