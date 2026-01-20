## Solana Finality → DEX Slippage Analytics (Pre/Post Alpenglow)

This project builds the foundation for a **Solana execution-quality dashboard** that measures how improvements in **network finality** translate into real trading outcomes—specifically **DEX slippage**—with a focus on comparing **before vs. after the Solana “Alpenglow” upgrade**.

### What’s in this repo

**1) Network performance baseline (2022–2026)**

* Tracks historical changes in Solana **block finality speed** and **network consistency**.
* Uses a pragmatic proxy for finality:

  * **Finality ≈ 31 blocks × average slot time**
* Produces visuals showing:

  * Finality time trend over years
  * Consistency trend (standard deviation of finality / lower is better)

**2) Data engineering pipeline notes**

* RPC-based ingestion at scale quickly hits rate limits on public endpoints.
* The ingestion layer is designed to be provider-agnostic but was implemented with:

  * **Helius RPC** for more reliable throughput under high request volume
  * **Birdeye API** for Raydium transaction-level coverage

**3) Raydium slippage module (in progress)**
Goal: compute slippage for pairs like **SOL–USDC** using:

* **slippage = (executed_price − expected_price) / expected_price**

Current status:

* ✅ Execution price can be collected reliably from Raydium swap/transaction data.
* ⚠️ The missing piece is a **reliable expected/quoted price at quote-time** (what the trader saw before execution). Without a timestamp-aligned quote signal, slippage can be conflated with market movement, making results unreliable.

### Why this matters

Finality improvements aren’t just “network stats”—they affect **execution certainty**, **latency/jitter**, and potentially **slippage distributions**, especially in fast markets and larger trade sizes. This repo is the baseline + pipeline groundwork to measure that impact rigorously before and after Alpenglow.

### What I’m looking for help with

If you’ve built slippage analytics on Solana, I’d love guidance on the most robust way to reconstruct **expected/quoted price** for Raydium swaps, e.g.:

* parsing program logs / instruction data to infer quote-time values
* replaying pre-trade pool state (reserves + fee model) at the correct slot
* integrating quote sources (Raydium/Jupiter) with reliable time alignment

### Planned roadmap

* [ ] Robust expected-price reconstruction (quote-time)
* [ ] Slippage distributions by trade size / volatility regime
* [ ] Pre/post Alpenglow comparison and dashboard integration
* [ ] Reproducible datasets + backtests

### Tech stack (typical)

* Python (Jupyter / notebooks)
* Solana RPC (Helius)
* Birdeye API (Raydium transactions)
* Data processing + visualization (pandas, matplotlib, etc.)


