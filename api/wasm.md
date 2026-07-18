# WASM

The WebAssembly build runs the same Rust core in the browser or any WASM runtime.
Construct a `FeatureStore` from a JSON spec and drive it with
`command(json) -> json`.

```bash
npm install wickra-feature-store-wasm
```

```javascript
import init, { FeatureStore } from 'wickra-feature-store-wasm'

await init() // fetches and instantiates the .wasm module

const spec = JSON.stringify({
  universe: ['AAA'],
  features: [{ kind: 'indicator', name: 'Sma', params: [2] }, { kind: 'price', field: 'close' }],
  labels: [{ kind: 'forward_return', horizon: 1 }],
})

const store = new FeatureStore(spec)
const data = { AAA: [{ ts: 0, open: 100, high: 100, low: 100, close: 100, volume: 1 }] }
const matrix = JSON.parse(store.command(JSON.stringify({ cmd: 'build_batch', data })))
console.log(matrix.columns)
```

The sequential WASM path produces a matrix byte-identical to the parallel native
build. See the [live demo](/demo) for the Wickra core running in your browser.

## More

- [npmjs.com/package/wickra-feature-store-wasm](https://www.npmjs.com/package/wickra-feature-store-wasm)
- [Source & examples](https://github.com/wickra-lib/wickra-feature-store/tree/main/bindings/wasm)
