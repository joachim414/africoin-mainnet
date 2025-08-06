# 🌍 Africoin – Africa’s Digital Currency Revolution

Africoin ($AFC) is the digital currency for real-world trade and financial freedom across all 54 African countries and beyond.

## 🔗 Smart Contract

- Standard: ERC-20
- Network: Polygon Mainnet
- Status: ✅ Deployed
- Contract address: `0xYourContractAddressHere` *(after deployment)*

## 🚀 Features

- Fast, low-fee transfers
- Wallet app for Android + Web
- Works across borders and currencies
- Designed to fight poverty, empower trade

## ⚙️ How to Deploy

Install dependencies:

```bash
npm install
PRIVATE_KEY=your_polygon_wallet_private_key
POLYGON_API=https://polygon-mainnet.infura.io/v3/your_project_id
npx hardhat run scripts/deploy-mainnet.js --network polygon
cd africoin-mainnet     # go to your local repo folder
git add README.md
git commit -m "Add Africoin project README"
git push
