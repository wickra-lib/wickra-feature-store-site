# About Wickra Feature Store

Wickra Feature Store folds OHLCV and microstructure event streams into ML-ready
feature/label matrices over the 514 streaming indicators of the Wickra core. A
build is a JSON document — **data, not code** — so the exact same feature build
runs in every one of ten languages and returns byte-for-byte identical results.

## What makes it different

- **The build is data.** A serde `FeatureSpec` — a `universe`, an ordered list of
  indicator / price / microstructure feature columns, and forward-looking label
  columns. Because it is data, it crosses the C ABI and WASM unchanged.
- **514 indicators.** Any feature column can reference any Wickra indicator (`Rsi`,
  `Ema`, `Macd`, `BollingerBands`, …), emitted one row per bar.
- **Forward-looking labels.** Forward returns over a horizon and triple-barrier
  labels are computed from future bars and aligned to the row that predicts them.
- **Parallel and deterministic.** The universe is folded in parallel with rayon, or
  sequentially on the WASM path — both produce a byte-identical matrix, pinned by a
  golden corpus in CI. Optional z-score / min-max column scaling is part of the spec.

## Why it exists

Feature engineering for market data is usually one language, one hard-coded
pipeline. Wickra Feature Store defines the feature surface **once**, in Rust, and
exposes it as a JSON-over-C-ABI data API to Rust, Python, Node.js, WASM and — over
a C ABI — C, C++, C#, Go, Java and R. The spec itself is portable JSON, so the same
pipeline runs anywhere, and columnar output (CSV, Arrow, Parquet) drops straight
into a training pipeline.

## Open source

Released under the **MIT OR Apache-2.0** license — permissive, OSI-approved, free
for any use including commercial. Source, issues and releases on
[GitHub](https://github.com/wickra-lib/wickra-feature-store).

## Disclaimer

Wickra Feature Store is a software library, **not** a trading system, and is
provided **as-is with no warranty**. It transforms market data into features; it
does not give financial advice. Use it at your own risk.
