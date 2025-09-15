UPDATED  OPEN-GATE APP  VERSION  2.O
/**
 * OPEN-GATE All-in-One Platform
 * Final Combined Code (Frontend + Backend Mockup)
 * Author: MARKBENX
 *
 * Features:
 * - Animated Landing (Login/Register with reptile effect)
 * - Trillion-Dollar Account Handling (Backend mockup)
 * - AI Wealth Detection
 * - Trading Bots placeholder
 * - Multi-Currency Support
 * - Analytics for Banks & Governments
 * - Stripe Live Checkout
 * - Fast Transactions
 * - Secure & Encrypted System
 * - Sound Effects Integration
 * - Hidden Admin Dashboard (for MARKBENX only)
 */

import React, { useState } from "react";
import { createRoot } from "react-dom/client";
import { motion } from "framer-motion";
import { loadStripe } from "@stripe/stripe-js";

const stripePromise = loadStripe("pk_test_12345"); // 🔑 Replace with real Stripe key

// 🔊 Sound helper
const playSound = (src) => {
  const audio = new Audio(src);
  audio.play();
};

// 🔐 Admin-only section
const AdminPanel = ({ logs }) => (
  <div className="bg-black text-green-400 p-6 rounded-lg shadow-lg mt-10">
    <h2 className="text-2xl mb-3 font-bold">🔐 SECRET ADMIN PANEL (MARKBENX)</h2>
    <p className="mb-4">All hidden financial logs & analytics are below:</p>
    <ul className="list-disc ml-6">
      {logs.map((log, i) => (
        <li key={i}>{log}</li>
      ))}
    </ul>
  </div>
);

// 🔮 AI Wealth Detection (Mockup)
const AIWealthDetection = () => (
  <div className="bg-purple-900 text-white p-6 mt-6 rounded-xl shadow-md">
    <h3 className="text-xl font-bold mb-2">🤖 AI Wealth Detection</h3>
    <p>Scanning accounts... detecting potential high-value assets securely.</p>
  </div>
);

// 🌍 Multi-Currency Support
const MultiCurrency = () => (
  <div className="bg-blue-900 text-white p-6 mt-6 rounded-xl shadow-md">
    <h3 className="text-xl font-bold mb-2">🌍 Multi-Currency Support</h3>
    <p>Any local currency is supported for transactions.</p>
  </div>
);

// 📊 Analytics for Banks
const AnalyticsBanks = () => (
  <div className="bg-green-900 text-white p-6 mt-6 rounded-xl shadow-md">
    <h3 className="text-xl font-bold mb-2">📊 Analytics for Banks & Governments</h3>
    <p>Big data processed for compliance, fraud detection & predictions.</p>
  </div>
);

function App() {
  const [user, setUser] = useState(null);
  const [logs, setLogs] = useState([]);

  // Fake login
  const handleLogin = () => {
    playSound("/sounds/click.mp3");
    setUser({ name: "MARKBENX", role: "admin" });
  };

  // Stripe payment handler
  const handleCheckout = async () => {
    playSound("/sounds/click.mp3");
    const stripe = await stripePromise;

    try {
      const res = await fetch("http://localhost:5000/create-checkout-session", {
        method: "POST",
      });
      const session = await res.json();

      playSound("/sounds/success.mp3");
      setLogs((prev) => [...prev, `💳 Payment started: ${session.id}`]);

      await stripe.redirectToCheckout({ sessionId: session.id });
    } catch (err) {
      console.error(err);
      playSound("/sounds/error.mp3");
    }
  };

  return (
    <div className="min-h-screen flex flex-col items-center justify-center bg-gradient-to-br from-black via-gray-900 to-black text-white">
      {/* Landing Title */}
      <motion.h1
        className="text-5xl font-extrabold mb-10"
        animate={{ x: [-15, 15, -15], color: ["#fff", "#22c55e", "#fff"] }}
        transition={{ repeat: Infinity, duration: 3 }}
      >
        🐉 OPEN-GATE | All-in-One Platform
      </motion.h1>

      {!user ? (
        <motion.button
          onClick={handleLogin}
          whileHover={{ scale: 1.1 }}
          className="bg-green-600 px-8 py-4 rounded-xl shadow-lg"
        >
          Login / Register
        </motion.button>
      ) : (
        <div className="flex flex-col items-center w-full px-10">
          <h2 className="text-2xl mb-6">Welcome, {user.name} 👑</h2>

          {/* Payment Button */}
          <motion.button
            onClick={handleCheckout}
            whileHover={{ scale: 1.1 }}
            className="bg-blue-600 hover:bg-blue-800 px-8 py-4 rounded-xl shadow-lg"
          >
            Pay with Stripe 💳
          </motion.button>

          {/* Feature Sections */}
          <AIWealthDetection />
          <MultiCurrency />
          <AnalyticsBanks />

          {/* Hidden Admin */}
          {user.role === "admin" && <AdminPanel logs={logs} />}
        </div>
      )}
    </div>
  );
}

// React Render
const root = createRoot(document.getElementById("root"));
root.render(<App />);

/* ===========================
   BACKEND MOCKUP (Node + Express)
   =========================== */

import express from "express";
import Stripe from "stripe";
import cors from "cors";

const app = express();
const stripe = new Stripe("sk_test_12345"); // Replace with secret key
app.use(cors());
app.use(express.json());

// Create Stripe checkout session
app.post("/create-checkout-session", async (req, res) => {
  try {
    const session = await stripe.checkout.sessions.create({
      payment_method_types: ["card"],
      line_items: [
        {
          price_data: {
            currency: "usd",
            product_data: { name: "OPEN-GATE Premium Access" },
            unit_amount: 10000, // $100
          },
          quantity: 1,
        },
      ],
      mode: "payment",
      success_url: "http://localhost:3000/success",
      cancel_url: "http://localhost:3000/cancel",
    });
    res.json({ id: session.id });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

app.listen(5000, () =>
  console.log("🚀 OPEN-GATE backend running on http://localhost:5000")
);
/***************************************************************

OPEN-GATE: Web3 / Crypto Wallet Support (MetaMask, WalletConnect, Binance)

Backend: signature verification + mock crypto invoice creation


Frontend: connect wallets, sign-in-with-wallet, show balance, send tx


Server: Add the routes to your existing Express server (server/index.js)

Client: Add the React component to your client and import/use it in App
***************************************************************/


/* ============================
SERVER: Add to server/index.js
(or create server/web3.js and require it from index.js)
============================ */
//
// Prereqs on server: npm i ethers express cors body-parser dotenv
//
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const { ethers } = require('ethers');
require('dotenv').config();

const web3Router = express.Router();
web3Router.use(cors());
web3Router.use(bodyParser.json());

// Example: server-side keep a nonce map for sign-in (in-memory for prototype)
const NONCES = {}; // { address: nonceString }

// Utility: generate nonce
function generateNonce() {
return OpenGate login nonce: ${Math.floor(Math.random() * 1000000000)} - ${Date.now()};
}

/**

GET /api/web3/nonce?address=0x...

Returns a nonce message the wallet should sign for authentication
*/
web3Router.get('/nonce', (req, res) => {
try {
const address = (req.query.address || '').toLowerCase();
if (!address) return res.status(400).json({ error: 'Missing address' });
const nonce = generateNonce();
NONCES[address] = nonce;
return res.json({ ok: true, nonce });
} catch (err) {
console.error('Nonce error', err);
return res.status(500).json({ error: 'Server error' });
}
});



/**

POST /api/web3/verify-signature

body: { address: "0x...", signature: "0x...", nonce: "..." }

Verifies the signature recovers the address


If valid, issues a server-side session token (JWT) OR returns success
*/
web3Router.post('/verify-signature', async (req, res) => {
try {
const { address, signature } = req.body;
if (!address || !signature) return res.status(400).json({ error: 'Missing params' });
const addr = address.toLowerCase();
const nonce = NONCES[addr];
if (!nonce) return res.status(400).json({ error: 'Nonce not found, request a new one' });


// Recover address from signature using ethers
const recovered = ethers.utils.verifyMessage(nonce, signature);
if (recovered.toLowerCase() !== addr) {
return res.status(401).json({ error: 'Signature mismatch' });
}

// Signature valid — create/return session token or user object
// For prototype, return a mock JWT-free response
// In production: create or look up user by wallet address and sign JWT
// Clean up nonce (prevent replay)
delete NONCES[addr];

// Example mock response:
return res.json({
ok: true,
address: addr,
message: 'Signature verified — wallet authenticated',
// token: "server-jwt-here"
});
} catch (err) {
console.error('Verify signature error', err);
return res.status(500).json({ error: 'Server error' });
}
});


/**

POST /api/web3/create-crypto-invoice

body: { amount: number, currency: "ETH"|"USDT"|..., toAddress?: optional recipient }

Creates a mock invoice for on-chain crypto payment (server-side record)


Return: { invoiceId, toAddress, amount, currency, qrData }
*/
const INVOICES = []; // In-memory invoice store for prototype



web3Router.post('/create-crypto-invoice', (req, res) => {
try {
const { amount, currency = 'ETH', toAddress } = req.body;
if (!amount) return res.status(400).json({ error: 'Missing amount' });

// For real: you could create an address per invoice (sub-wallet) or use an on-chain monitoring service  
// For prototype we use a constant receiver or provided toAddress  
const receiver = toAddress || (process.env.CRYPTO_RECEIVER_ADDRESS || '0x0000000000000000000000000000000000000000');  

const invoice = {  
  id: `inv_${Date.now()}_${Math.floor(Math.random()*9999)}`,  
  amount,  
  currency,  
  receiver,  
  status: 'pending',  
  createdAt: Date.now(),  
};  
INVOICES.push(invoice);  

// Prepare QR data for wallets (EIP-681 / ethereum URI)  
// Example URI: ethereum:0x..address.. ?value=0.1&gas=... (value in ETH)  
// For ERC20 you'd present token and value calculate decimals; here we show ETH simple URI  
let qrData = '';  
if (currency === 'ETH') {  
  // ethers expects value in wei - but for URI pass decimal  
  qrData = `ethereum:${receiver}?value=${amount}`;  
} else {  
  // For tokens, include token transfer page or instruct user to pay exact token amount to receiver  
  qrData = `pay:${receiver}:${amount} ${currency}`;  
}  

return res.json({ ok: true, invoice: invoice, qrData });

} catch (err) {
console.error('Create invoice error', err);
return res.status(500).json({ error: 'Server error' });
}
});

/**

GET /api/web3/invoice/:id/status

Returns invoice status
*/
web3Router.get('/invoice/:id/status', (req, res) => {
const id = req.params.id;
const inv = INVOICES.find(i => i.id === id);
if (!inv) return res.status(404).json({ error: 'Invoice not found' });
return res.json({ ok: true, invoice: inv });
});



/* mount the router in your main server file:
const web3Routes = require('./web3Routes'); app.use('/api/web3', web3Routes);
Or if appended directly, use app.use('/api/web3', web3Router)
*/
module.exports = web3Router;

/* ============================
In your server/main index.js, after creating 'app' do:
const web3Router = require('./path/to/this/file');
app.use('/api/web3', web3Router);
============================ */

/* ============================
CLIENT: React component (client/src/Web3Wallet.jsx)

Put this file in your React app and import it in your App.jsx

Requires: ethers, @walletconnect/web3-provider (optional)
============================ */
import React, { useEffect, useState } from 'react';
import { ethers } from 'ethers';
import WalletConnectProvider from "@walletconnect/web3-provider"; // optional
// If you prefer Web3Modal or wagmi, you can swap in those libraries for more UX features


const API = process.env.REACT_APP_API || 'http://localhost:4002';

export default function Web3Wallet() {
const [provider, setProvider] = useState(null); // ethers provider
const [signer, setSigner] = useState(null);
const [address, setAddress] = useState('');
const [chainId, setChainId] = useState(null);
const [balance, setBalance] = useState(null);
const [invoices, setInvoices] = useState([]);
const [status, setStatus] = useState('');

// Connect MetaMask (EVM)
async function connectMetaMask() {
if (!window.ethereum) {
alert('MetaMask not installed. Suggest installing MetaMask or use WalletConnect.');
return;
}
try {
// Request accounts
await window.ethereum.request({ method: 'eth_requestAccounts' });
const tempProvider = new ethers.providers.Web3Provider(window.ethereum);
const tempSigner = tempProvider.getSigner();
const addr = await tempSigner.getAddress();
const network = await tempProvider.getNetwork();
setProvider(tempProvider);
setSigner(tempSigner);
setAddress(addr);
setChainId(network.chainId);
loadBalance(tempProvider, addr);
// listen for account changes
window.ethereum.on('accountsChanged', (accounts) => {
if (accounts.length === 0) {
disconnectWallet();
} else {
setAddress(accounts[0]);
loadBalance(tempProvider, accounts[0]);
}
});
window.ethereum.on('chainChanged', (_chainId) => window.location.reload());
} catch (err) {
console.error('MetaMask connect err', err);
alert('MetaMask connection error: ' + (err.message || err));
}
}

// Connect WalletConnect (mobile)
async function connectWalletConnect() {
try {
const wcProvider = new WalletConnectProvider({
rpc: { 1: "https://mainnet.infura.io/v3/YOUR_INFURA_KEY" }, // add other networks as needed
qrcode: true,
});
// Enable session (triggers QR Code modal)
await wcProvider.enable();
const tempProvider = new ethers.providers.Web3Provider(wcProvider);
const tempSigner = tempProvider.getSigner();
const addr = await tempSigner.getAddress();
setProvider(tempProvider);
setSigner(tempSigner);
setAddress(addr);
loadBalance(tempProvider, addr);
// wcProvider.on("disconnect", ...) handle as needed
} catch (err) {
console.error('WalletConnect error', err);
alert('WalletConnect error: ' + err.message);
}
}

// Detect Binance Chain Wallet (often injected as window.BinanceChain)
async function connectBinanceChain() {
try {
const bc = window.BinanceChain;
if (!bc) {
alert('Binance Chain Wallet not installed');
return;
}
await bc.request({ method: 'eth_requestAccounts' });
const tempProvider = new ethers.providers.Web3Provider(bc);
const tempSigner = tempProvider.getSigner();
const addr = await tempSigner.getAddress();
setProvider(tempProvider);
setSigner(tempSigner);
setAddress(addr);
loadBalance(tempProvider, addr);
} catch (err) {
console.error('Binance connect err', err);
}
}

// Generic disconnect
function disconnectWallet() {
setProvider(null); setSigner(null); setAddress(''); setBalance(null);
setStatus('Wallet disconnected');
}

// Load ETH balance
async function loadBalance(p, addr) {
try {
const bal = await p.getBalance(addr);
setBalance(ethers.utils.formatEther(bal));
} catch (err) {
console.error('Load balance err', err);
}
}

// Sign-in with wallet: sign server-provided nonce
async function signInWithWallet() {
try {
setStatus('Requesting nonce from server...');
// 1. ask server for nonce for this address
const lower = address.toLowerCase();
const nonceResp = await fetch(${API}/api/web3/nonce?address=${lower});
const nonceData = await nonceResp.json();
if (!nonceData || !nonceData.nonce) {
alert('Could not retrieve nonce from server');
return;
}
const nonce = nonceData.nonce;

// 2. sign the nonce with wallet  
  setStatus('Requesting signature in wallet...');  
  const signature = await signer.signMessage(nonce);  

  // 3. submit signature to server for verification  
  setStatus('Verifying signature on server...');  
  const verifyResp = await fetch(`${API}/api/web3/verify-signature`, {  
    method: 'POST',  
    headers: { 'Content-Type': 'application/json' },  
    body: JSON.stringify({ address: lower, signature }),  
  });  
  const verifyData = await verifyResp.json();  
  if (verifyData.ok) {  
    setStatus('Wallet verified — logged in');  
    // Optionally store server JWT returned here to localStorage  
  } else {  
    setStatus('Verification failed: ' + (verifyData.error || 'unknown'));  
  }  
} catch (err) {  
  console.error('Sign-in error', err);  
  setStatus('Sign-in error: ' + (err.message || err));  
}

}

// Create crypto invoice server-side
async function createCryptoInvoice(amount, currency='ETH') {
try {
setStatus('Creating invoice...');
const resp = await fetch(${API}/api/web3/create-crypto-invoice, {
method: 'POST',
headers: { 'Content-Type': 'application/json' },
body: JSON.stringify({ amount, currency }),
});
const data = await resp.json();
if (data.ok) {
setInvoices(prev => [data.invoice, ...prev]);
return data.invoice;
} else {
alert('Invoice creation failed: ' + (data.error || 'unknown'));
}
} catch (err) {
console.error('Create invoice error', err);
}
}

// Send ETH payment from connected wallet to invoice receiver (via signer)
async function payInvoice(invoice) {
try {
if (!signer) { alert('Connect wallet first'); return; }
setStatus('Sending transaction from wallet...');
// For ETH: send plain transaction
if (invoice.currency === 'ETH') {
const tx = await signer.sendTransaction({
to: invoice.receiver,
value: ethers.utils.parseEther(invoice.amount.toString()),
});
setStatus('Transaction sent: ' + tx.hash);
// Optionally wait for confirmation
await tx.wait();
setStatus('Payment confirmed on-chain: ' + tx.hash);
// Notify server (mock) to mark invoice paid (for prototyping)
// fetch(${API}/api/web3/invoice/${invoice.id}/paid, { method:'POST' })
} else {
// For ERC-20 tokens: need token contract interaction (approve/transferFrom not applicable)
// Example: send ERC-20 via contract's transfer function
// (User must have token & connected to token network)
alert('Token payments: implement ERC-20 transfer using token contract ABI and signer');
}
// Refresh balance
loadBalance(provider, address);
} catch (err) {
console.error('Pay invoice error', err);
setStatus('Payment failed: ' + (err.message || err));
}
}

// Example: get ERC-20 token balance (for USDT example)
async function loadTokenBalance(tokenAddress) {
if (!provider || !address) return;
// Minimal ERC20 ABI
const abi = ["function balanceOf(address) view returns (uint256)", "function decimals() view returns (uint8)"];
const contract = new ethers.Contract(tokenAddress, abi, provider);
const raw = await contract.balanceOf(address);
const decimals = await contract.decimals().catch(()=>18);
const human = ethers.utils.formatUnits(raw, decimals);
return human;
}

useEffect(()=> {
// refresh balance periodically (optional)
let t;
if (provider && address) {
t = setInterval(()=> loadBalance(provider, address), 15_000);
}
return ()=> clearInterval(t);
}, [provider, address]);

return (
<div className="p-4 bg-white rounded shadow text-black max-w-3xl">
<h3 className="text-xl font-bold mb-2">Crypto Wallet (Web3) — OPEN-GATE</h3>

<div className="flex gap-3 mb-3">  
    <button onClick={connectMetaMask} className="bg-yellow-400 px-3 py-2 rounded">Connect MetaMask</button>  
    <button onClick={connectWalletConnect} className="bg-indigo-600 text-white px-3 py-2 rounded">Connect WalletConnect</button>  
    <button onClick={connectBinanceChain} className="bg-gray-700 text-white px-3 py-2 rounded">Connect Binance Wallet</button>  
    <button onClick={disconnectWallet} className="bg-red-500 text-white px-3 py-2 rounded">Disconnect</button>  
  </div>  

  <div className="mb-3">  
    <strong>Address:</strong> {address || 'Not connected'}  
  </div>  
  <div className="mb-3">  
    <strong>Network chainId:</strong> {chainId || '—'}  
  </div>  
  <div className="mb-3">  
    <strong>ETH Balance:</strong> {balance || '—'}  
  </div>  

  <div className="mt-4">  
    <button onClick={signInWithWallet} className="bg-green-600 text-white px-4 py-2 rounded mr-2">Sign-in with Wallet</button>  
    <button onClick={async ()=> {  
      const inv = await createCryptoInvoice(0.01, 'ETH');  
      if (inv) {  
        // Show and attempt payment  
        const confirmed = window.confirm(`Invoice ${inv.id} created. Pay ${inv.amount} ${inv.currency}?`);  
        if (confirmed) payInvoice(inv);  
      }  
    }} className="bg-blue-600 text-white px-4 py-2 rounded">Create Invoice (0.01 ETH)</button>  
  </div>  

  <div className="mt-4">  
    <div className="text-sm text-gray-600">Status: {status}</div>  
  </div>  

  <div className="mt-4">  
    <h4 className="font-semibold">Invoices</h4>  
    <ul>  
      {invoices.map(inv => (  
        <li key={inv.id} className="border p-2 my-2">  
          <div>ID: {inv.id}</div>  
          <div>Amount: {inv.amount} {inv.currency}</div>  
          <div>Receiver: {inv.receiver}</div>  
          <div>Status: {inv.status}</div>  
          <div className="mt-2">  
            <button onClick={() => payInvoice(inv)} className="px-3 py-1 bg-blue-500 text-white rounded">Pay invoice</button>  
          </div>  
        </li>  
      ))}  
    </ul>  
  </div>  
</div>

);
}

/* ============================
Client usage notes:

Import and render <Web3Wallet /> in your App.jsx where you want wallet UI.

Use environment variable REACT_APP_API to point to server (ex: http://localhost:4002)

Ensure CORS is enabled on server for these routes.

For production: handle JWT creation on the server after verifying signature
and return to client for authenticated API usage.
============================ */


/***************************************************************

SECURITY / PROD NOTES (READ)

NEVER use in-memory NONCES/INVOICES for production; persist them to DB.


Always use HTTPS.


Protect endpoints: rate-limit the /nonce and /verify-signature endpoints to prevent abuse.


Use server-side JWT creation after verifying signature and use JWT to secure other APIs.


Crypto payments are irreversible — implement on-chain monitoring & confirmations.


For token/ERC-20 transfers use token contract ABI and appropriate decimals.


To accept many tokens, maintain mapping of token address <-> decimals <-> symbol.


For multiplicity of networks (BSC, Polygon, etc) configure rpc endpoints per chainId.


WalletConnect requires a bridge or provider config (Infura/Alchemy/RPC endpoints).


*************************************************************/// === OPEN-GATE FULL FEATURE PROJECT (Without animated landing/login) ===
// Tech: React + Node/Express + MongoDB + Stripe/PayPal/MPesa + i18next + Three.js
// Includes: Marketplace, 3D products, sound, dropshipping, global currency, multilingual, admin dashboard, AI-ready


// ================= BACKEND: server.js =================
import express from "express";
import mongoose from "mongoose";
import cors from "cors";
import Stripe from "stripe";
import bodyParser from "body-parser";
import dotenv from "dotenv";

dotenv.config();
const app = express();
app.use(cors());
app.use(bodyParser.json());

// MongoDB connect
mongoose.connect(process.env.MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true })
.then(()=>console.log("MongoDB Connected"))
.catch(err=>console.error(err));

// Models
const ProductSchema = new mongoose.Schema({
name:String,
price:Number,
currency:String,
images:[String],
category:String,
threeDModel:String,
stock:Number,
supplierAPI:String // for dropshipping
});
const OrderSchema = new mongoose.Schema({
userId:String,
products:Array,
total:Number,
currency:String,
status:{type:String,default:"pending"},
paymentMethod:String
});

const Product = mongoose.model("Product",ProductSchema);
const Order = mongoose.model("Order",OrderSchema);

// Stripe Payment
const stripe = new Stripe(process.env.STRIPE_SECRET);

app.post("/api/pay", async(req,res)=>{
try{
const { amount, currency, token } = req.body;
const payment = await stripe.paymentIntents.create({
amount: amount * 100,
currency,
payment_method: token,
confirm:true
});
res.json({ success:true, payment });
}catch(err){ res.status(500).json({ success:false, error:err.message }); }
});

// Multi-currency (auto convert)
app.get("/api/convert", async(req,res)=>{
const { amount, from, to } = req.query;
// dummy conversion (replace with forex API)
const rate = 1.2;
res.json({ converted: amount * rate, to });
});

// Dropshipping supplier integration (dummy)
app.get("/api/supplier/products", async(req,res)=>{
// Simulate pulling products from AliExpress/Shopify API
const supplierProducts = [
{ id:"sup1", name:"Smart Watch", price:20, currency:"USD", supplier:"AliExpress" },
{ id:"sup2", name:"Shoes", price:15, currency:"USD", supplier:"Alibaba" }
];
res.json(supplierProducts);
});

// Admin stats & fraud detection
app.get("/api/admin/stats", async(req,res)=>{
const orders = await Order.find();
const totalRevenue = orders.filter(o=>o.status==="completed")
.reduce((sum,o)=>sum+o.total,0);
const suspicious = orders.filter(o=>o.total>10000); // dummy fraud detection
res.json({ orders:orders.length, revenue:totalRevenue, suspicious:suspicious.length });
});

app.listen(5000,()=>console.log("Server running on 5000"));

// ================= FRONTEND: App.jsx =================
import React, {useState, useEffect} from "react";
import { BrowserRouter, Routes, Route } from "react-router-dom";
import { Canvas } from "@react-three/fiber";
import { OrbitControls } from "@react-three/drei";
import i18n from "i18next";
import { initReactI18next, useTranslation } from "react-i18next";

// === Languages ===
i18n.use(initReactI18next).init({
resources:{
en:{translation:{welcome:"Welcome to OPEN-GATE", buy:"Buy"}},
fr:{translation:{welcome:"Bienvenue à OPEN-GATE", buy:"Acheter"}},
es:{translation:{welcome:"Bienvenido a OPEN-GATE", buy:"Comprar"}},
zh:{translation:{welcome:"欢迎来到 OPEN-GATE", buy:"购买"}}
},
lng:"en", fallbackLng:"en", interpolation:{escapeValue:false}
});

// === Sound Hook ===
const useSound = (url) => {
const play = ()=>{ new Audio(url).play(); };
return play;
};

// === 3D Product Viewer ===
const Product3D = () => (
<Canvas style={{height:"200px"}}>
<ambientLight intensity={0.5}/>
<spotLight position={[10,10,10]}/>
<OrbitControls/>
<mesh>
<sphereGeometry args={[1,32,32]}/>
<meshStandardMaterial color="blue"/>
</mesh>
</Canvas>
);

// === Marketplace ===
const Marketplace = () => {
const {t} = useTranslation();
const [products,setProducts] = useState([]);
const playSound = useSound("/sounds/add-to-cart.mp3");
useEffect(()=>{
fetch("/api/supplier/products").then(r=>r.json()).then(setProducts);
},[]);
return (
<div className="p-6 grid grid-cols-1 md:grid-cols-3 gap-6">
{products.map(p=>(
<div key={p.id} className="bg-white p-4 rounded-lg shadow">
<h2 className="font-bold">{p.name}</h2>
<p>{p.price} {p.currency}</p>
<Product3D/>
<button onClick={playSound} className="bg-green-500 text-white px-4 py-2 rounded mt-2">{t("buy")}</button>
</div>
))}
</div>
);
};

// === Admin Dashboard ===
const Admin = () => {
const [stats,setStats] = useState({});
useEffect(()=>{ fetch("/api/admin/stats").then(r=>r.json()).then(setStats)},[]);
return (
<div className="p-6 bg-gray-100">
<h1 className="text-3xl font-bold">Admin Dashboard</h1>
<div className="grid grid-cols-3 gap-4 mt-4">
<div className="bg-white p-4 rounded shadow">Orders: {stats.orders}</div>
<div className="bg-white p-4 rounded shadow">Revenue: {stats.revenue}</div>
<div className="bg-red-200 p-4 rounded shadow">Suspicious Orders: {stats.suspicious}</div>
</div>
</div>
);
};

// === App Routes ===
const App = () => {
const {t,i18n} = useTranslation();
return (
<BrowserRouter>
<div className="p-4 bg-indigo-600 text-white flex justify-between">
<h1 className="text-xl font-bold">{t("welcome")}</h1>
<select onChange={e=>i18n.changeLanguage(e.target.value)} className="text-black">
<option value="en">EN</option>
<option value="fr">FR</option>
<option value="es">ES</option>
<option value="zh">中文</option>
</select>
</div>
<Routes>
<Route path="/" element={<Marketplace/>}/>
<Route path="/admin" element={<Admin/>}/>
</Routes>
</BrowserRouter>
);
};

export default App;

// ================= NOTES =================
// - No landing/login included (as requested).
// - Sound effects added for add-to-cart.
// - Multi-language + multi-currency demo included.
// - Dropshipping supplier API placeholder included.
// - Admin dashboard expanded with fraud detection.
// - Extendable for crypto/Web3 wallets./********************************************************************
OPEN-GATE SYSTEM (BACKEND + BASIC FRONTEND)
With:

1. Environment Setup


2. Backend (Node.js + Express)


3. MongoDB for Orders


4. Automated Email + WhatsApp Messages


5. Integrated Chatbot API


6. React Frontend Example (Checkout + Chatbot)
********************************************************************/



/***********************

(1) ENVIRONMENT SETUP
***********************/
// In .env file, put your secrets like this:
// EMAIL_USER=Morgandemaxkeq@gmail.com
// EMAIL_PASS=your_gmail_app_password
// TWILIO_SID=your_twilio_sid
// TWILIO_TOKEN=your_twilio_auth_token
// TWILIO_PHONE=your_twilio_whatsapp_number
// MONGO_URI=your_mongodb_uri


/***********************

(2) BACKEND: EXPRESS APP
***********************/
import express from "express";
import mongoose from "mongoose";
import nodemailer from "nodemailer";
import twilio from "twilio";
import bodyParser from "body-parser";
import cors from "cors";
import dotenv from "dotenv";
dotenv.config();


const app = express();
app.use(cors());
app.use(bodyParser.json());

/***********************

(3) DATABASE (Orders)
***********************/
const orderSchema = new mongoose.Schema({
name: String,
email: String,
phone: String,
product: String,
amount: Number,
status: { type: String, default: "Pending" },
});


const Order = mongoose.model("Order", orderSchema);

/***********************

(4) EMAIL + WHATSAPP
***********************/
const transporter = nodemailer.createTransport({
service: "gmail",
auth: {
user: process.env.EMAIL_USER,
pass: process.env.EMAIL_PASS,
},
});


const twilioClient = twilio(process.env.TWILIO_SID, process.env.TWILIO_TOKEN);

async function sendOrderNotifications(order) {
// Email
await transporter.sendMail({
from: "OPEN-GATE" <${process.env.EMAIL_USER}>,
to: order.email,
subject: "Order Confirmation - OPEN-GATE",
text: Hello ${order.name}, your order for ${order.product} has been received. Amount: $${order.amount}. Thank you for shopping with OPEN-GATE!,
});

// WhatsApp
await twilioClient.messages.create({
from: whatsapp:${process.env.TWILIO_PHONE},
to: whatsapp:${order.phone},
body: ✅ Hello ${order.name}, your order for ${order.product} has been confirmed. Amount: $${order.amount}. Stay tuned for updates from OPEN-GATE.,
});
}

/***********************

(5) CHATBOT API
***********************/
app.post("/api/chat", async (req, res) => {
const { message } = req.body;


// Simple rule-based chatbot (expand later with AI)
let reply = "I'm here to help. Can you be more specific?";
if (message.toLowerCase().includes("order")) {
reply = "To check your order, go to Dashboard → My Orders.";
} else if (message.toLowerCase().includes("pay")) {
reply = "We accept payments via PayPal, Stripe, Mpesa, and Binance Pay.";
} else if (message.toLowerCase().includes("hello")) {
reply = "Hi 👋, welcome to OPEN-GATE support!";
}

res.json({ reply });
});

/***********************

(6) ORDER API
***********************/
app.post("/api/orders", async (req, res) => {
try {
const newOrder = new Order(req.body);
await newOrder.save();

// Trigger email + WhatsApp
await sendOrderNotifications(newOrder);

res.json({ success: true, message: "Order placed & notifications sent!" });
} catch (err) {
res.status(500).json({ success: false, message: err.message });
}
});


/***********************

SERVER START
***********************/
mongoose
.connect(process.env.MONGO_URI)
.then(() => {
app.listen(5000, () =>
console.log("🚀 OPEN-GATE backend running on http://localhost:5000")
);
})
.catch((err) => console.error(err));


/***********************

(7) FRONTEND EXAMPLE (React Checkout + Chatbot)
**********************/
// Example React component
/
import React, { useState } from "react";
import axios from "axios";


function Checkout() {
const [form, setForm] = useState({ name: "", email: "", phone: "", product: "", amount: "" });
const [chat, setChat] = useState("");
const [reply, setReply] = useState("");

const handleOrder = async () => {
await axios.post("http://localhost:5000/api/orders", form);
alert("Order placed! Confirmation sent to email + WhatsApp.");
};

const handleChat = async () => {
const res = await axios.post("http://localhost:5000/api/chat", { message: chat });
setReply(res.data.reply);
};

return (
<div>
<h2>Checkout</h2>
<input placeholder="Name" onChange={(e) => setForm({ ...form, name: e.target.value })} />
<input placeholder="Email" onChange={(e) => setForm({ ...form, email: e.target.value })} />
<input placeholder="Phone (+254...)" onChange={(e) => setForm({ ...form, phone: e.target.value })} />
<input placeholder="Product" onChange={(e) => setForm({ ...form, product: e.target.value })} />
<input placeholder="Amount" type="number" onChange={(e) => setForm({ ...form, amount: e.target.value })} />
<button onClick={handleOrder}>Place Order</button>

<h2>Chatbot</h2>  
  <input placeholder="Ask something..." onChange={(e) => setChat(e.target.value)} />  
  <button onClick={handleChat}>Send</button>  
  <p>{reply}</p>  
</div>

);
}

export default Checkout;
*/ combine these code orderly  without  altering
anything  the generate this code the way it  orderly

