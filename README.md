# CTSA Pro — Crypto Technical Signal Dashboard

![Version](https://img.shields.io/badge/version-2.0-22c55e?style=flat-square)
![Status](https://img.shields.io/badge/status-live-22c55e?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-6366f1?style=flat-square)
![Data](https://img.shields.io/badge/data-Binance%20API-orange?style=flat-square)
![Scanner](https://img.shields.io/badge/scanner-Cloudflare%20Workers-F38020?style=flat-square)
![Telegram](https://img.shields.io/badge/sinyal-@ctsaprosignals-2CA5E0?style=flat-square)

**CTSA Pro** adalah web app analisis teknikal kripto yang menghasilkan sinyal trading **BELI** dan **JUAL (Early Warning)** secara otomatis berdasarkan sistem Hard Filter + Scoring berlapis — dilengkapi scanner otomatis yang mengirimkan sinyal ke Telegram setiap 30 menit.

🌐 **Live Demo:** [https://nemnem25.github.io/CTSAPro/](https://nemnem25.github.io/CTSAPro/)  
📡 **Telegram:** [@ctsaprosignals](https://t.me/ctsaprosignals)

---

## Filosofi

> *"Lebih sedikit sinyal yang akurat lebih baik dari banyak sinyal yang noise."*

CTSA Pro tidak mengejar kuantitas sinyal. Sistem dirancang berlapis — hanya sinyal yang lolos semua hard filter **dan** mencapai skor minimum yang dikirimkan ke Telegram. Sinyal lemah diblokir secara otomatis.

---

## Arsitektur Sistem v2.0

```
Binance API
    │
    ▼
Cloudflare Worker (scanner.js)
    │── Fetch klines 1H + 4H + 1D (29 coin)
    │── Compute BELI signal
    │── Compute JUAL signal
    │── Hard Filter + Scoring + Penalty
    │── Anti-duplikat (KV Store)
    ▼
Telegram @ctsaprosignals          GitHub Pages (index.html)
   🟢 BELI KUAT                      Web App — Realtime
   🔴 JUAL KUAT                      Dashboard + Chart
```

---

## Dual Signal System

### Sinyal BELI
Entry long untuk spot atau derivatif. Dikirim saat kondisi bullish terkonfirmasi.

### Sinyal JUAL — Early Warning
**Bukan short, bukan jual aset yang dipegang.** Sinyal JUAL adalah:
- ⚠️ Early warning — pasar sedang turun, jangan beli dulu
- 🎯 Timing akumulasi — identifikasi level beli berikutnya
- 📉 Pelengkap sinyal BELI — konteks lengkap kondisi pasar

---

## Hard Filter

Semua filter **wajib lolos** sebelum scoring dimulai. Satu filter gagal = sinyal tidak dikirim.

### Hard Filter BELI (6 syarat)

| # | Filter | Kondisi |
|---|--------|---------|
| 1 | BTC Context | BTC > EMA50 1D |
| 2 | Price Trend | Harga coin > EMA50 1H |
| 3 | Momentum | ADX > 20 |
| 4 | No Conflict | SellScore ≤ 1 |
| 5 | Breakout | Close > MAX high 20 candle + volume > MA |
| 6 | Retest Hold | Bounce di breakout level (window 10 candle) |

### Hard Filter JUAL (5 syarat)

| # | Filter | Kondisi |
|---|--------|---------|
| 1 | Price Trend | Harga coin < EMA50 1H |
| 2 | Momentum | ADX > 20 |
| 3 | No Conflict | BuyScore ≤ 1 |
| 4 | Breakdown | Close < MIN low 20 candle + volume > MA |
| 5 | Retest Fail | Rejection di breakdown level (window 10 candle) |

> **Catatan:** BTC bearish (BTC < EMA50 1D) tidak memblokir sinyal JUAL — tapi masuk sebagai **bonus scoring +1**.

---

## Scoring System

### Scoring BELI (max 10 poin)

| Aspek | Kondisi | Poin |
|-------|---------|------|
| Breakout valid | Close > MAX high 20 candle | +1 |
| Retest Hold | Bounce di breakout level | +1 |
| RSI ideal | RSI 55–68 | +1 |
| MACD Cross Up | Histogram cross positif | +1 |
| Volume | Volume > MA20 | +1 |
| ADX kuat | ADX > 25 | +1 |
| Struktur HH/HL | Higher High + Higher Low | +1 |
| Trend coin | EMA50 > EMA200 (1H) | +1 |
| MTF 4H | EMA50 > EMA200 di 4H | +1 |
| MTF 1D | EMA50 > EMA200 di 1D | +1 |

**Penalty BELI:** RSI > 70 (−1), Dekat resistance < 1 ATR (−2), Volume lemah (−1)

### Scoring JUAL (max 11 poin)

| Aspek | Kondisi | Poin |
|-------|---------|------|
| Breakdown valid | Close < MIN low 20 candle | +1 |
| Retest Fail | Rejection di breakdown level | +1 |
| RSI ideal | RSI 32–45 | +1 |
| MACD Cross Down | Histogram cross negatif | +1 |
| Volume | Volume > MA20 | +1 |
| ADX kuat | ADX > 25 | +1 |
| Struktur LL/LH | Lower Low + Lower High | +1 |
| Trend coin | EMA50 < EMA200 (1H) | +1 |
| BTC bearish | BTC < EMA50 1D | +1 |
| MTF 4H | EMA50 < EMA200 di 4H | +1 |
| MTF 1D | EMA50 < EMA200 di 1D | +1 |

**Penalty JUAL:** RSI < 30 (−1), Dekat support < 1 ATR (−2), Volume lemah (−1)

---

## Verdict & Threshold

| Skor | BELI | JUAL | Dikirim? |
|------|------|------|----------|
| ≥ 8 | 🟢 BELI KUAT | 🔴 JUAL KUAT | ✅ Ya |
| 6–7 | 🟢 BELI | 🔴 JUAL | ✅ Ya |
| 5 | BELI LEMAH | JUAL LEMAH | ❌ Tidak |
| < 5 | TUNGGU | TUNGGU | ❌ Tidak |

---

## Level Trading

### BELI
```
Entry Limit = Harga − 0.5× ATR   (lebih optimal)
Stop Loss   = Harga − 1.5× ATR
Target 1    = Harga + 3.0× ATR   R/R = 2.0
Target 2    = Harga + 6.0× ATR
Target 3    = Harga + 9.0× ATR
Target 4    = Harga + 12.0× ATR
Target 5    = Harga + 15.0× ATR
```

### JUAL (Early Warning)
```
Invalidasi  = Harga + 1.5× ATR   (SL jika trading short)
Target 1    = Harga − 3.0× ATR
Target 2    = Harga − 6.0× ATR
Target 3    = Harga − 9.0× ATR
Target 4    = Harga − 12.0× ATR
Target 5    = Harga − 15.0× ATR
Zona Akumulasi = antara Target 1 dan Target 2
```

---

## Format Sinyal Telegram

### 🟢 BELI
```
🟢 CTSA PRO SIGNAL v2.0

SOL/USDT — BELI KUAT
Skor: 8/10 (min 6) · Semua filter ✅
📈 SPOT BELI · DERIVATIF LONG

💰 Harga: $88.50
🔓 Breakout: $85.00 ← Baru breakout
⏳ Konfirmasi close 1H: 19 Apr, 14.00 WIB

── KONFIRMASI TIMEFRAME ──
✅ 1H: BELI KUAT (8/10)
✅ 4H: Bullish (EMA50 > EMA200)
❌ 1D: Belum bullish

── LEVEL TRADING ──
🎯 Entry Market: $88.50
🎯 Entry Limit: $88.00 (lebih optimal)
🛑 Stop Loss: $86.25 (-2.54%) · 1.5× ATR
✅ Target 1: $93.00 (+5.08%) · R/R 2.0
...
```

### 🔴 JUAL
```
🔴 CTSA PRO SIGNAL v2.0

SOL/USDT — JUAL KUAT
Skor: 8/10 (min 6) · Semua filter ✅
📉 EARLY WARNING · HARGA TURUN

💰 Harga: $88.50
🔻 Breakdown: $90.00 ← Retest gagal

── LEVEL HARGA ──
⚠️ Jangan beli di atas: $90.00
🛑 Invalidasi warning: $90.75 (+2.54%)
🎯 Zona akumulasi: $81.00 — $84.75

── POTENSI PENURUNAN ──
📉 Target 1: $84.75 (-4.24%)
...
```

---

## 6 Indikator Inti

### 1. RSI — Relative Strength Index (14)
**Kategori:** Momentum

Mengukur kecepatan dan besarnya perubahan harga. Zona ideal entry BELI: 55–68. Zona ideal early warning JUAL: 32–45.

**Bukti akademis:**
- Wilder, J.W. (1978). *New Concepts in Technical Trading Systems*. Trend Research.
- PMC/NIH (2023): Strategi berbasis RSI menghasilkan return 773.65% dari 2018–2022 vs buy-and-hold 275.22%.
- arXiv (2024): RSI14 masuk dalam top 8 fitur prediksi Bitcoin menggunakan XGBoost.

### 2. MACD (12, 26, 9)
**Kategori:** Trend & Momentum

Cross Up histogram = konfirmasi bullish. Cross Down = konfirmasi bearish.

**Bukti akademis:**
- ScienceDirect (2024): Kombinasi MACD + LSTM menghasilkan win rate tinggi pada 18 saham.
- Gate.io Research (2026): Kombinasi RSI + MACD mencapai win rate 77% dalam backtesting kripto.

### 3. Bollinger Bands (20, 2σ)
**Kategori:** Volatilitas

BB Squeeze mengindikasikan breakout akan segera terjadi. Posisi harga dalam band menentukan kondisi overbought/oversold.

### 4. ADX (14)
**Kategori:** Kekuatan Trend

ADX > 20 = trend ada (syarat hard filter). ADX > 25 = trend kuat (scoring +1). ADX < 20 = sideways, sinyal diblokir.

**Bukti akademis:**
- 73% professional forex traders menggunakan ADX (Taylor & Allen, 2012).
- arXiv (2024): ADX masuk fitur penting prediksi Bitcoin pada studi XGBoost.

### 5. OBV — On-Balance Volume
**Kategori:** Volume

Divergensi OBV terhadap harga adalah sinyal peringatan reversal yang kuat — sering mendahului pergerakan harga aktual.

### 6. Fear & Greed Index — Dual Source
**Kategori:** Sentimen Pasar

Dua sumber: Alternative.me + CoinMarketCap. Jika selisih ≥ 20 poin (divergen), F&G diabaikan dari scoring. Tidak masuk scoring — hanya konteks informasi.

---

## 29 Coin yang Di-scan

| Tier | Coin |
|------|------|
| Original | BTC, ETH, BNB, SOL, XRP, ADA, AVAX, DOT, LINK, DOGE, MATIC, UNI, ATOM, LTC, NEAR |
| Tier 1 | SUI, APT, OP, ARB, INJ |
| Tier 2 | FET, RNDR, TIA, SEI, WLD |
| Tier 3 | PEPE, BONK, WIF, FLOKI |

---

## Tech Stack

| Komponen | Teknologi |
|----------|-----------|
| Frontend | HTML5, CSS3, Vanilla JavaScript |
| Charts | Chart.js 4.4.1 |
| Data Harga | Binance API |
| Proxy | Cloudflare Workers (bypass CORS/ISP) |
| Scanner | Cloudflare Workers + Cron Triggers |
| KV Storage | Cloudflare KV (anti-duplikat sinyal) |
| Telegram | Bot API — @ctsaprosignals |
| Hosting | GitHub Pages |
| Font | System fonts |

---

## Scanner Endpoints

| Endpoint | Fungsi |
|----------|--------|
| `/scan` | Trigger scan manual semua 29 coin |
| `/positions` | Lihat posisi aktif di KV |
| `/log` | Log sinyal terbaru |
| `/weekly` | Ringkasan performa mingguan |
| `/test` | Test koneksi Telegram |
| `/reset` | Reset semua posisi aktif |

**Base URL:** `https://ctsa-scanner.mahapalamultimedia.workers.dev`

---

## Referensi Akademis

| Penulis | Tahun | Judul | Sumber |
|---------|-------|-------|--------|
| Wilder, J.W. | 1978 | New Concepts in Technical Trading Systems | Trend Research |
| Appel, G. | 1979 | The Moving Average Convergence-Divergence Method | Great Neck |
| Granville, J. | 1963 | Granville's New Key to Stock Market Profits | Prentice-Hall |
| Bollinger, J. | 2001 | Bollinger on Bollinger Bands | McGraw-Hill |
| Murphy, J.J. | 1999 | Technical Analysis of the Financial Markets | NYIF |
| Baker & Wurgler | 2006 | Investor Sentiment and the Cross-Section of Stock Returns | Journal of Finance |
| Taylor & Allen | 2012 | The use of technical analysis in the foreign exchange market | Journal of International Money and Finance |
| PMC/NIH | 2023 | Cryptocurrency trading strategies based on RSI | PubMed Central |
| ScienceDirect | 2024 | Technical indicator empowered intelligent strategies (MACD+LSTM) | ScienceDirect |
| arXiv | 2024 | Predicting Bitcoin Market Trends with XGBoost | arXiv:2410.06935 |
| JMSR | 2025 | Bollinger Bands, RSI and MACD vs Fundamental Analysis | JMSR |
| Tandfonline | 2026 | A Hybrid SVR-Based Framework for Cryptocurrency Price Forecasting | Tandfonline |

---

## Roadmap

- [x] v1.0 — UI dashboard + arsitektur dasar
- [x] v1.1 — Koneksi real-time Binance API
- [x] v1.2 — Kalkulasi 6 indikator otomatis di browser
- [x] v1.3 — ADX threshold refinement + F&G global note
- [x] v2.0 — Hard Filter system + Breakout + Retest + ATR-based SL/TP
- [x] v2.0 — Scanner otomatis Cloudflare + Telegram @ctsaprosignals
- [x] v2.0 — Dual Signal System (BELI + JUAL Early Warning)
- [x] v2.0 — Multi-Timeframe scoring (1H + 4H + 1D)
- [x] v2.0 — 29 coin (Original 15 + Tier 1/2/3)
- [x] v2.0 — Anti-duplikat via KV + reversal detection
- [ ] v2.1 — VPVR approximation di scanner
- [ ] v3.0 — VPS migration + realtime websocket + lebih banyak coin

---

## Disclaimer

> ⚠️ **CTSA Pro bukan saran investasi.**
>
> Semua sinyal yang dihasilkan adalah output matematis dari indikator teknikal historis. Trading kripto mengandung risiko tinggi. Selalu lakukan riset independen (DYOR) sebelum membuat keputusan investasi. Past performance tidak menjamin hasil masa depan.

---

*CTSA Pro · nemnem25 · 2026*
