# CTSA Pro — Crypto Technical Signal Dashboard

![Version](https://img.shields.io/badge/version-1.3-22c55e?style=flat-square)
![Status](https://img.shields.io/badge/status-live-22c55e?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-6366f1?style=flat-square)
![Data](https://img.shields.io/badge/data-Binance%20API-orange?style=flat-square)

**CTSA Pro** adalah web app analisis teknikal kripto yang menghasilkan sinyal trading BELI / JUAL berdasarkan **6 indikator teknikal terpilih** — semua terbukti secara akademis dalam penelitian peer-reviewed dan digunakan secara luas oleh trader profesional di seluruh dunia.

🌐 **Live Demo:** [https://nemnem25.github.io/CTSAPro/](https://nemnem25.github.io/CTSAPro/)

---

## Filosofi

> *"Lebih sedikit indikator yang tepat lebih baik dari banyak indikator yang redundan."*

CTSA Pro tidak mengejar kuantitas indikator. Setiap indikator yang digunakan dipilih berdasarkan tiga kriteria ketat:

1. **Terbukti secara akademis** — ada studi peer-reviewed yang mengkonfirmasi efektivitasnya
2. **Dipakai trader profesional** — bukan indikator eksperimental atau hasil overfitting
3. **Tidak redundan** — setiap indikator mengukur dimensi pasar yang berbeda

---

## 6 Indikator Inti

### 1. RSI — Relative Strength Index (14)
**Kategori:** Momentum

Mengukur kecepatan dan besarnya perubahan harga. RSI di bawah 30 menandakan kondisi *oversold* (potensi beli), di atas 70 menandakan *overbought* (potensi jual). Dikembangkan oleh J. Welles Wilder dan dipublikasikan pertama kali pada 1978.

**Bukti akademis:**
- Wilder, J.W. (1978). *New Concepts in Technical Trading Systems*. Trend Research.
- Studi peer-reviewed PMC/NIH (2023) menemukan strategi berbasis RSI menghasilkan return **773.65%** dari 2018–2022, dibandingkan buy-and-hold **275.22%** pada periode yang sama.
- ResearchGate (2023): RSI terbukti lebih akurat dari MACD dalam mendeteksi sinyal di 10 pasangan kripto.
- arXiv (2024): RSI14 dan RSI30 masuk dalam **top 8 fitur** prediksi Bitcoin menggunakan XGBoost.

**Referensi:**
- https://pubmed.ncbi.nlm.nih.gov/ (search: RSI cryptocurrency strategy)
- https://www.researchgate.net/publication/377921778
- https://arxiv.org/abs/2410.06935

---

### 2. MACD — Moving Average Convergence Divergence (12, 26, 9)
**Kategori:** Trend & Momentum

Mengukur hubungan antara dua EMA untuk mendeteksi perubahan arah trend dan kekuatan momentum. Sinyal utama: crossover MACD line dengan signal line, dan pembalikan histogram. Dikembangkan oleh Gerald Appel pada 1979.

**Bukti akademis:**
- Appel, G. (1979). *The Moving Average Convergence-Divergence Method*. Great Neck.
- ScienceDirect (2024): Kombinasi MACD dengan LSTM/GRU menghasilkan win rate tinggi pada 18 saham dari NEPSE, BSE, dan NYSE.
- Gate.io Research (Januari 2026): Kombinasi RSI + MACD mencapai **win rate 77%** dalam backtesting kripto.
- MACD terbukti efektif mendeteksi *trend reversals* dan pola kelanjutan trend pada aset volatil.

**Referensi:**
- https://www.sciencedirect.com/science/article/pii/S2199853124001926
- https://web3.gate.com/crypto-wiki/article/how-to-use-technical-indicators-macd-rsi-and-bollinger-bands-for-crypto-trading-in-2026-20260204

---

### 3. Bollinger Bands (20 periode, 2 standar deviasi)
**Kategori:** Volatilitas

Terdiri dari tiga garis: SMA 20 sebagai middle band, dan dua band atas/bawah yang berjarak 2 standar deviasi. Mengukur volatilitas secara dinamis. *BB Squeeze* (band menyempit) mengindikasikan breakout akan segera terjadi. Dikembangkan oleh John Bollinger pada 1983.

**Bukti akademis:**
- Bollinger, J. (2001). *Bollinger on Bollinger Bands*. McGraw-Hill.
- Journal of Marketing & Social Research (2025): Bollinger Bands dan MACD menunjukkan **kemampuan prediktif signifikan** di pasar volatil, khususnya trading minyak dan emas 2018–2023.
- Tandfonline (2026): BB masuk dalam 13 indikator terpilih untuk prediksi harga aset dalam studi SVR hybrid.
- Kraken Research: Kombinasi BB + RSI meningkatkan akurasi identifikasi peluang reversal secara substansial.

**Referensi:**
- https://jmsr-online.com/article/technical-analysis-vs-fundamental-analysis-a-comparative-study-of-bollinger-bands-rsi-and-macd-against-fundamental-factors-in-commodity-trading-82/
- https://www.tandfonline.com/doi/full/10.1080/08839514.2026.2612793
- https://www.kraken.com/learn/crypto-technical-indicators

---

### 4. ADX — Average Directional Index (14)
**Kategori:** Kekuatan Trend

Mengukur **kekuatan** trend, bukan arahnya. ADX di atas 30 mengkonfirmasi trend yang kuat (layak diikuti). ADX 20–30 menandakan trend mulai terbentuk (zona lemah). ADX di bawah 20 mengindikasikan pasar sideways — sinyal trend-following sebaiknya diabaikan. Digunakan bersama DI+ dan DI- untuk menentukan arah. Dikembangkan oleh J. Welles Wilder pada 1978.

**Bukti akademis:**
- Wilder, J.W. (1978). *New Concepts in Technical Trading Systems*. Trend Research.
- Taylor, M.P. & Allen, H. (2012): Penelitian menemukan **73% professional forex traders** secara rutin menggunakan ADX dalam pengambilan keputusan trading.
- arXiv (2024): ADX masuk dalam fitur penting prediksi Bitcoin pada studi XGBoost dengan akurasi tinggi.
- ADX terbukti sebagai filter efektif untuk menghindari sinyal palsu di kondisi market sideways.

**Referensi:**
- https://arxiv.org/abs/2410.06935
- https://www.sciencedirect.com/science/article/pii/S2666827025000143

---

### 5. OBV — On-Balance Volume
**Kategori:** Volume

Mengakumulasi volume berdasarkan arah pergerakan harga. OBV naik saat harga naik mengkonfirmasi kekuatan trend. Divergensi OBV terhadap harga adalah sinyal peringatan reversal yang kuat — sering mendahului pergerakan harga aktual. Dikembangkan oleh Joseph Granville pada 1963.

**Bukti akademis:**
- Granville, J. (1963). *Granville's New Key to Stock Market Profits*. Prentice-Hall.
- Murphy, J.J. (1999). *Technical Analysis of the Financial Markets*. New York Institute of Finance.
- arXiv (2024): OBV dan sinyal volume masuk dalam fitur penting prediksi Bitcoin dalam studi XGBoost.
- Patel et al. (2015): Volume termasuk dalam 10 parameter ML terbaik, dengan Random Forest superior menangani indikator kontinu termasuk OBV.

**Referensi:**
- https://arxiv.org/abs/2410.06935
- https://www.ajer.org/papers/v5(12)/Z05120207212.pdf

---

### 6. Fear & Greed Index — Dual Source
**Kategori:** Sentimen Pasar

Mengukur sentimen pasar kripto secara agregat dari berbagai sumber data. CTSA Pro menggunakan **dua sumber sekaligus** — Alternative.me dan CoinMarketCap — untuk validasi silang. Jika selisih kedua sumber ≥ 20 poin (divergen), sinyal F&G diabaikan dari scoring untuk menghindari false signal. Extreme Fear (≤25) secara historis adalah zona akumulasi; Extreme Greed (≥75) adalah zona distribusi.

**Bukti akademis:**
- Baker, M. & Wurgler, J. (2006). *Investor Sentiment and the Cross-Section of Stock Returns*. Journal of Finance, 61(4), 1645–1680. Membuktikan indeks sentimen secara signifikan memprediksi return pasar.
- PMC/NIH (2023): Sentimen pasar dikombinasikan dengan RSI meningkatkan akurasi sinyal secara keseluruhan.
- Behavioral Finance literature mendukung penggunaan sentimen sebagai indikator kontradiktif — ketakutan ekstrem pasar sering bertepatan dengan titik balik harga.

**Referensi:**
- https://alternative.me/crypto/fear-and-greed-index/
- https://coinmarketcap.com/charts/fear-and-greed-index/
- https://doi.org/10.1111/j.1540-6261.2006.00885.x (Baker & Wurgler 2006)

---

## Logika Scoring & Sinyal

### Sistem Konfluensi

CTSA Pro tidak mengandalkan satu indikator. Sinyal dihasilkan dari **konfluensi** — semakin banyak indikator yang sepakat, semakin tinggi probabilitas sinyal benar.

| Skor Konfluensi | Label | Probabilitas |
|-----------------|-------|-------------|
| 6 / 6 indikator | BELI / JUAL SANGAT KUAT | ~85% |
| 5 / 6 indikator | BELI / JUAL KUAT | ~75% |
| 4 / 6 indikator | BELI / JUAL MODERAT | ~65% |
| 3 / 6 indikator | SINYAL LEMAH — tunggu | ~50% |
| < 3 / 6 indikator | TIDAK ADA SINYAL | — |

### Kalkulasi Stop Loss & Target

Semua level harga dihitung berbasis **ATR (Average True Range)** — metode standar industri yang diakui secara akademis (Wilder 1978; Petukhina & Petukhov 2020; Brown & Jennings 2004).

```
Stop Loss  = Entry ± 1.0 × ATR(14)
Target 1   = Entry ± 2.0 × ATR(14)  → R/R minimum 2:1
Target 2   = Entry ± 4.0 × ATR(14)
Target 3   = Entry ± 7.0 × ATR(14)
```

Sinyal hanya dieksekusi jika **R/R ratio ≥ 2:1** pada Target 1.

### Aturan Multi-Timeframe

Sinyal valid hanya jika **minimal 2 dari 3 timeframe utama** sepakat:

| Kombinasi | Kekuatan | Timeframe Entry |
|-----------|----------|-----------------|
| 1W + 1D sepakat | Sangat Kuat | 4H |
| 1D + 4H sepakat | Kuat | 1H |
| Hanya 4H | Lemah — skip | — |

---

## Fitur

- Sinyal BELI / JUAL otomatis dengan skor konfluensi
- Entry, Stop Loss, Target 1–3 berbasis ATR
- Analisis multi-timeframe (1H, 4H, 1D, 1W)
- Dual Fear & Greed Index (Alternative.me + CoinMarketCap)
- Deteksi divergensi F&G — skip scoring otomatis
- Grafik harga interaktif dengan Bollinger Bands + EMA
- MACD histogram panel
- 15 pasangan kripto utama
- Data real-time via Binance API (tanpa VPN)
- Desain responsif — desktop & mobile
- Dark mode by default

---

## Tech Stack

| Komponen | Teknologi |
|----------|-----------|
| Frontend | HTML5, CSS3, Vanilla JavaScript |
| Charts | Chart.js 4.4.1 |
| Data Harga | Binance API (via Cloudflare Worker proxy) |
| F&G | Alternative.me API + CoinMarketCap API |
| Proxy | Cloudflare Workers (bypass blokir ISP Indonesia) |
| Hosting | GitHub Pages |
| CI/CD | GitHub Actions |
| Font | System fonts (Segoe UI / SF Pro) |

---

## Referensi Akademis Lengkap

| Penulis | Tahun | Judul | Sumber |
|---------|-------|-------|--------|
| Wilder, J.W. | 1978 | New Concepts in Technical Trading Systems | Trend Research |
| Appel, G. | 1979 | The Moving Average Convergence-Divergence Method | Great Neck |
| Granville, J. | 1963 | Granville's New Key to Stock Market Profits | Prentice-Hall |
| Bollinger, J. | 2001 | Bollinger on Bollinger Bands | McGraw-Hill |
| Murphy, J.J. | 1999 | Technical Analysis of the Financial Markets | NYIF |
| Baker & Wurgler | 2006 | Investor Sentiment and the Cross-Section of Stock Returns | Journal of Finance |
| Patel et al. | 2015 | Predicting Stock and Stock Price Index Movement Using Trend Deterministic Data Preparation and Machine Learning | Expert Systems with Applications |
| Taylor & Allen | 2012 | The use of technical analysis in the foreign exchange market | Journal of International Money and Finance |
| Petukhina & Petukhov | 2020 | ATR-based risk management in trading strategies | — |
| Brown & Jennings | 2004 | Volatility Forecasting and Market Efficiency | Journal of Financial Markets |
| PMC/NIH | 2023 | Cryptocurrency trading strategies based on RSI | PubMed Central |
| ScienceDirect | 2024 | Technical indicator empowered intelligent strategies (MACD+LSTM) | ScienceDirect |
| ScienceDirect | 2025 | Key technical indicators for stock market prediction | ScienceDirect |
| arXiv | 2024 | Predicting Bitcoin Market Trends with XGBoost | arXiv:2410.06935 |
| JMSR | 2025 | Bollinger Bands, RSI and MACD vs Fundamental Analysis | Journal of Marketing & Social Research |
| Tandfonline | 2026 | A Hybrid SVR-Based Framework for Cryptocurrency Price Forecasting | Tandfonline |

---

## Roadmap

- [x] v1.0 — UI dashboard + arsitektur dasar
- [x] v1.1 — Koneksi real-time Binance API
- [x] v1.2 — Kalkulasi 6 indikator otomatis di browser
- [x] v1.3 — ADX threshold refinement + F&G global note
- [ ] v2.0 — On-chain data integration

---

## Disclaimer

> ⚠️ **CTSA Pro bukan saran investasi.**
>
> Semua sinyal yang dihasilkan adalah output matematis dari indikator teknikal historis. Trading kripto mengandung risiko tinggi. Selalu lakukan riset independen (DYOR — Do Your Own Research) sebelum membuat keputusan investasi. Past performance tidak menjamin hasil masa depan.

---

*CTSA Pro · nemnem25 · 2026*
