# HQL Smart Money Structure
### by [Hafzan Quant Labs](https://github.com/HafzanQuantLabs)

![Pine Script](https://img.shields.io/badge/Pine%20Script-v5-1E88E5?style=for-the-badge&logo=tradingview&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-TradingView-131722?style=for-the-badge&logo=tradingview&logoColor=white)
![Version](https://img.shields.io/badge/Version-1.0-00D9FF?style=for-the-badge)
![License](https://img.shields.io/badge/License-Proprietary-FF1744?style=for-the-badge)

> Precision buy & sell signals with multi-timeframe trend analysis, BOS & CHoCH detection, divergence scanning, market profile, liquidity zones, and a real-time trend strength matrix — engineered for institutional-minded traders.

---

## 📸 Preview

> *Add a screenshot of the indicator on a chart here*

---

## ✨ Features

### 🟢 🔴 Buy & Sell Signals
Precision entry signals generated only when multiple filters align simultaneously — momentum, higher timeframe trend, lower timeframe confirmation, volume, and breakout conditions all pass before a signal fires.

### 🏗️ Market Structure — BOS & CHoCH
Automatic detection of **Break of Structure (BOS)** and **Change of Character (CHoCH)** on both bullish and bearish sides, drawn directly on the chart with color-coded lines and zones.

| Signal | Color | Meaning |
|---|---|---|
| CHoCH Sell | 🔵 Cyan | Bearish character change |
| CHoCH Buy | 🟢 Green | Bullish character change |
| BOS Sell | 🟣 Purple | Bearish structure break |
| BOS Buy | 🩵 Teal | Bullish structure break |

### 📊 7-Timeframe Trend Strength Matrix
Real-time trend direction across **1M, 5M, 15M, 30M, 1H, 4H, and 1D** displayed in a live dashboard table. Combines EMA(20) and VWAP to determine trend bias per timeframe.

- **Trend Strength Score** — aggregated score from -100 to +100
- **System Confidence** — 50% / 60% / 75% / 90% based on alignment
- **CVD** — Cumulative Volume Delta showing net buying/selling pressure

### 🔮 Trend Prediction Matrix
Per-timeframe prediction scores calculated from trend bias, momentum, and volatility. Displayed as ▲ / ▼ / ━ in the bottom-right panel across 5M → 1D.

### ⚡ RSI Divergence Scanner
Automatically detects **bullish** and **bearish** RSI divergences using a 3-point comparison method.
- Bullish divergence: price making lower lows while RSI makes higher lows (RSI < 40)
- Bearish divergence: price making higher highs while RSI makes lower highs (RSI > 60)

### 🔥 Market Profile Analysis
Identifies institutional order flow imbalances by comparing buy vs sell volume ratios against a 20-bar average. Fires `🔥 BUY` and `🔥 SELL` labels when strong directional flow is detected.

### 💧 Liquidity Zone Detection
Flags price levels near recent highs and lows where liquidity is likely resting — potential targets for sweeps and reversals.

### 📈 Dynamic Trendlines
Automatically draws support and resistance trendlines using short and long trend period lookbacks. Color-coded by trend strength — from strong green to strong red.

### 🎯 TP & SL Lines
Every BUY and SELL signal automatically draws dashed **Take Profit** (green) and **Stop Loss** (red) lines on the chart based on your configured point values.

### ⚠️ Get Ready Signals
Optional early-warning signals that fire when price is approaching but hasn't yet hit the full momentum threshold — giving you time to prepare.

---

## ⚙️ Settings

### General
| Setting | Default | Description |
|---|---|---|
| Pivot Length | 5 | Bars used to identify pivot highs and lows |
| Base Momentum Threshold | 0.01% | Minimum price change required for a signal |
| Take Profit (points) | 10 | TP distance from entry in points |
| Stop Loss (points) | 10 | SL distance from entry in points |
| Min Signal Distance | 5 bars | Minimum bars between consecutive signals |
| Short Trend Period | 30 | Lookback for short trendline |
| Long Trend Period | 100 | Lookback for long trendline |

### Signal Filters
| Filter | Default | Description |
|---|---|---|
| Momentum Filter | ✅ On | Require price change to exceed threshold |
| Higher TF Trend Filter | ✅ On | Require alignment with selected higher TF |
| Higher Timeframe | 5M | Timeframe for HTF trend confirmation |
| Lower TF Filter | ✅ On | Block signals against lower TF trend |
| Lower Timeframe | 5M | Timeframe for LTF confirmation |
| Volume Filter | ✅ On | Require above-average volume |
| Breakout Filter | ✅ On | Require price to break recent high/low |
| Show Get Ready | ❌ Off | Show early warning signals |
| Restrict Repeated Signals | ✅ On | Prevent repeated signals in same trend |

### Advanced Analysis Tools
| Tool | Default | Description |
|---|---|---|
| Liquidity Zone Detection | ❌ Off | Show liquidity pool labels |
| Market Profile Analysis | ✅ On | Show institutional flow labels |
| Divergence Scanner | ✅ On | Show RSI divergence labels |
| Trend Strength Matrix | ✅ On | Show prediction table |

---

## 🚀 Installation

1. Open [TradingView](https://www.tradingview.com) and navigate to the **Pine Script Editor**
2. Click **New** to open a blank script
3. Copy the full contents of [`hql_sms.pine`](./hql_sms.pine)
4. Paste into the editor and click **Save**
5. Click **Add to chart**

---

## 💡 How to Use

**For scalping / short-term:**
- Set Higher TF to `15M` or `30M`
- Set Lower TF to `1M` or `5M`
- Keep momentum threshold low (0.005–0.01)

**For swing trading:**
- Set Higher TF to `4H` or `D`
- Set Lower TF to `1H`
- Increase momentum threshold (0.02–0.05)

**Signal quality tips:**
- Highest confidence signals occur when the Trend Strength table shows 5+ timeframes aligned in one direction
- Wait for Confidence to show 75% or 90% before taking signals
- Use BOS/CHoCH levels as confluence — signals near these levels carry more weight

---

## 🔔 Alerts

Set alerts directly in TradingView by right-clicking the indicator → **Add Alert**.

Available conditions:
- `🟢 BUY` signal fired
- `🔴 SELL` signal fired
- `CHoCH` bullish / bearish detected
- `BOS` bullish / bearish detected
- `⚡ BULL` divergence detected
- `⚡ BEAR` divergence detected

---

## 🛠️ Technical Architecture

| Component | Implementation |
|---|---|
| Security calls | Consolidated to 7 calls (down from 31) using tuple returns |
| Trend detection | EMA(20) + VWAP confluence per timeframe |
| ATR | Fallback to `high - low` when ATR is unavailable |
| Trendlines | Run on `barstate.isconfirmed` to prevent tick-level recalculation |
| Label management | Capped at 500 with `max_labels_count` |
| CHoCH/BOS | Uses `close` crossover/crossunder of pivot levels |
| Pivot tracking | Maintains `last` and `prev` pivot values for accurate BOS detection |

---

## 📋 Changelog

### v1.0
- Initial release
- 7-timeframe trend matrix
- BOS & CHoCH detection
- Buy/sell signals with TP/SL lines
- RSI divergence scanner
- Market profile analysis
- Liquidity zone detection
- Dynamic trendlines
- Trend prediction matrix

---

## ⚠️ Disclaimer

This indicator is for **educational and informational purposes only**. It does not constitute financial advice. Past performance is not indicative of future results. Trading involves significant risk of loss. Always conduct your own research and use proper risk management before making any trading decisions.

---

## 📄 License

© Hafzan Quant Labs. All Rights Reserved.
Unauthorized copying, modification, distribution, or commercial use is strictly prohibited without explicit written permission from Hafzan Quant Labs.

---

<div align="center">

**[Hafzan Quant Labs](https://github.com/HafzanQuantLabs)** — Quantitative tools for serious traders.

</div>
