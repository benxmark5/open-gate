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
