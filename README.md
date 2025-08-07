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
// components/CurrencyConverter.tsx

import React, { useEffect, useState } from 'react';
import { View, Text, TextInput, Picker, ActivityIndicator } from 'react-native';
import * as Localization from 'expo-localization'; // for detecting user locale
import axios from 'axios';

const CurrencyConverter = () => {
  const [currency, setCurrency] = useState('USD'); // default
  const [amount, setAmount] = useState('');
  const [converted, setConverted] = useState(null);
  const [loading, setLoading] = useState(false);
  const [rates, setRates] = useState({});

  const supportedCurrencies = ['USD', 'EUR', 'ZAR', 'NGN', 'KES', 'EGP', 'GBP'];

  // Get user locale currency code
  useEffect(() => {
    const userCurrency = Localization.currency || 'USD';
    if (supportedCurrencies.includes(userCurrency)) {
      setCurrency(userCurrency);
    }
  }, []);

  // Fetch live conversion rates (USD to AFC, NGN to AFC, etc.)
  useEffect(() => {
    const fetchRates = async () => {
      setLoading(true);
      try {
        const res = await axios.get('https://api.exchangerate.host/latest?base=USD'); // Free API
        const afcPriceUSD = 0.10; // Example AFC price in USD
        let newRates: any = {};

        supportedCurrencies.forEach(cur => {
          const rate = res.data.rates[cur];
          newRates[cur] = afcPriceUSD / (rate || 1);
        });

        setRates(newRates);
      } catch (err) {
        console.error('Failed to fetch rates', err);
      } finally {
        setLoading(false);
      }
    };

    fetchRates();
  }, []);

  // Convert input amount to AFC
  const handleConvert = (value: string) => {
    setAmount(value);
    const rate = rates[currency] || 0;
    setConverted((parseFloat(value) / rate).toFixed(2));
  };

  return (
    <View className="p-4 bg-white rounded-2xl shadow">
      <Text className="text-lg font-bold">Buy Africoin</Text>

      <Text className="mt-2">Enter Amount</Text>
      <TextInput
        className="border rounded p-2 mt-1 mb-2"
        placeholder={`Amount in ${currency}`}
        keyboardType="numeric"
        value={amount}
        onChangeText={handleConvert}
      />

      <Text>Select Currency</Text>
      <Picker
        selectedValue={currency}
        onValueChange={setCurrency}
        style={{ height: 50 }}
      >
        {supportedCurrencies.map((cur) => (
          <Picker.Item key={cur} label={cur} value={cur} />
        ))}
      </Picker>

      {loading ? (
        <ActivityIndicator className="mt-2" />
      ) : (
        <Text className="mt-3 text-lg">
          🔁 You’ll receive: <Text className="font-bold">{converted || 0} AFC</Text>
        </Text>
      )}
    </View>
  );
};

export default CurrencyConverter;
import { Linking, Button } from 'react-native';

const openTransakWidget = (walletAddress: string, selectedCurrency: string) => {
  const transakURL = `https://global.transak.com?apiKey=YOUR_TRANSAK_API_KEY&walletAddress=${walletAddress}&fiatCurrency=${selectedCurrency}&cryptoCurrency=AFC&network=polygon&disableWalletAddressForm=true`;

  Linking.openURL(transakURL);
};

// Example Buy button in your UI:
<Button
  title="Buy with Transak"
  onPress={() => {
    if (!walletAddress) {
      alert('Please connect your wallet first');
      return;
    }
    openTransakWidget(walletAddress, currency);
  }}
/>
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract Africoin is ERC20 {
    constructor() ERC20("Africoin", "AFC") {
        _mint(msg.sender, 100000000 * 10 ** decimals());
    }
}
async function main() {
  const Africoin = await ethers.getContractFactory("Africoin");
  const token = await Africoin.deploy();
  await token.deployed();

  console.log("Africoin deployed to:", token.address);
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
Africoin deployed to: 0xABC123456789abcdef...
