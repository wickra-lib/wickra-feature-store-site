---
layout: home
title: Wickra Feature Store — ML-ready feature matrices from market data
titleTemplate: false

hero:
  name: "Wickra Feature Store"
  text: "One spec. A feature matrix."
  tagline: "Fold OHLCV and microstructure event streams into ML-ready feature/label matrices over 514 streaming indicators. A build is a JSON FeatureSpec — data, not code — deterministic and byte-identical across ten languages."
  image:
    src: /wickra-mark.svg
    alt: Wickra Feature Store
  actions:
    - theme: brand
      text: View on GitHub
      link: https://github.com/wickra-lib/wickra-feature-store
    - theme: alt
      text: Features & labels
      link: https://github.com/wickra-lib/wickra-feature-store/blob/main/docs/FEATURES.md
    - theme: alt
      text: API
      link: /api/rust

features:
  - icon: 🧱
    title: The build is JSON, not code
    details: A build is a serde FeatureSpec — a universe, an ordered list of indicator / price / microstructure feature columns, and forward-looking label columns. Because it is data, the exact same feature build crosses the C ABI and WASM unchanged.
  - icon: 📈
    title: 514 indicators
    details: "Every feature column can reference any of the 514 O(1) streaming indicators from the Wickra core — Rsi, Ema, Macd, BollingerBands and the rest — emitted one row per bar."
  - icon: 🏷️
    title: Forward-looking labels
    details: "Join supervised targets to each row — forward returns over a horizon, or triple-barrier labels — computed from future bars and aligned to the feature row that predicts them."
  - icon: 🌐
    title: Ten languages, one matrix
    details: "The core is a JSON-over-C-ABI data API (FeatureStore::command) in Rust, Python, Node.js, WASM, C, C++, C#, Go, Java and R. A developer in any language builds the same matrix."
  - icon: ⚡
    title: Parallel, or streaming
    details: "The universe folds in parallel with rayon, or sequentially on the WASM fallback path; the same core also streams one bar at a time. All three produce a byte-for-byte identical matrix."
  - icon: 🧪
    title: Deterministic, proven
    details: The parallel and sequential paths are byte-identical, pinned by a golden corpus replayed through all ten bindings in CI. No RNG, stable column order, ties broken by symbol key.
---

<script setup>
const installTabs = [
  { label: 'Python', lang: 'bash', code: 'pip install wickra-feature-store' },
  { label: 'Node',   lang: 'bash', code: 'npm install wickra-feature-store' },
  { label: 'Rust',   lang: 'bash', code: 'cargo add wickra-feature-store' },
  { label: 'WASM',   lang: 'bash', code: 'npm install wickra-feature-store-wasm' },
  { label: 'C',      lang: 'bash', code: '# prebuilt header + library from GitHub releases:\n# github.com/wickra-lib/wickra-feature-store/releases' },
  { label: 'C#',     lang: 'bash', code: 'dotnet add package Wickra.FeatureStore' },
  { label: 'Go',     lang: 'bash', code: 'go get github.com/wickra-lib/wickra-feature-store-go' },
  { label: 'Java',   lang: 'xml',  code: '<!-- Maven Central -->\n<dependency>\n  <groupId>org.wickra</groupId>\n  <artifactId>wickra-feature-store</artifactId>\n  <version>0.1.3</version>\n</dependency>' },
  { label: 'R',      lang: 'r',    code: 'install.packages("wickrafeaturestore", repos = "https://wickra-lib.r-universe.dev")' },
]

const pyCode = `import json
from wickra_feature_store import FeatureStore

spec = json.dumps({
    "universe": ["AAA"],
    "features": [
        {"kind": "indicator", "name": "Sma", "params": [2]},
        {"kind": "price", "field": "close"},
    ],
    "labels": [{"kind": "forward_return", "horizon": 1}],
})

store = FeatureStore(spec)
data = {"AAA": [{"ts": 0, "open": 100, "high": 100, "low": 100, "close": 100, "volume": 1}]}
response = store.command(json.dumps({"cmd": "build_batch", "data": data}))
matrix = json.loads(response)
print(matrix["columns"])`

const nodeCode = `import { FeatureStore } from 'wickra-feature-store'

const spec = JSON.stringify({
  universe: ['AAA'],
  features: [
    { kind: 'indicator', name: 'Sma', params: [2] },
    { kind: 'price', field: 'close' },
  ],
  labels: [{ kind: 'forward_return', horizon: 1 }],
})

const store = new FeatureStore(spec)
const data = { AAA: [{ ts: 0, open: 100, high: 100, low: 100, close: 100, volume: 1 }] }
const matrix = JSON.parse(store.command(JSON.stringify({ cmd: 'build_batch', data })))
console.log(matrix.columns)`

const cliCode = `# Build a matrix from a directory of <SYMBOL>.csv candle files:
wickra-feature-store --spec spec.json --data ./data              # JSON to stdout
wickra-feature-store --spec spec.json --data ./data --format csv
wickra-feature-store --spec spec.json --data ./data --format parquet --out features.parquet`

const snippetTabs = [
  { label: 'Python', lang: 'python',     code: pyCode },
  { label: 'Node',   lang: 'javascript', code: nodeCode },
  { label: 'CLI',    lang: 'bash',       code: cliCode },
]
</script>

## The build is JSON, not code

A build is a `FeatureSpec`: a `universe`, an ordered list of `features`, and an
optional list of `labels`. Feature columns are price fields, indicators or
microstructure statistics; label columns are forward-looking targets joined to
each row.

```json
{
  "universe": ["AAA", "BBB", "CCC"],
  "features": [
    { "kind": "indicator", "name": "Rsi", "params": [14] },
    { "kind": "indicator", "name": "Ema", "params": [20] },
    { "kind": "price", "field": "close" }
  ],
  "labels": [
    { "kind": "forward_return", "horizon": 5 },
    { "kind": "triple_barrier", "horizon": 20, "upper": 2.0, "lower": 1.0 }
  ],
  "scaling": "z_score"
}
```

## Install

The same feature build from every language — native Rust, Python, Node.js and
WASM, plus a C ABI for C, C++, C#, Go, Java and R.

<InstallTabs :tabs="installTabs" />

## Run it from any language

Construct a `FeatureStore` from the JSON spec, then drive it with
`command(json) -> json`. Every binding returns the same bytes.

<InstallTabs :tabs="snippetTabs" />

## Built on the Wickra core

Wickra Feature Store is part of the [Wickra](https://wickra.org) ecosystem. Every
feature column draws on the 514 O(1) streaming indicators of
[`wickra-core`](https://github.com/wickra-lib/wickra), so a matrix sees exactly
the same numbers a backtest or a live chart would.

> Wickra Feature Store is a software library, not a trading system, and comes with
> no warranty — use at your own risk.
