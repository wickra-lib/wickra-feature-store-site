# Node.js

The Node package is a native napi addon over the Rust core. Construct a
`FeatureStore` from a JSON spec and drive it with `command(json) -> json`.

```bash
npm install wickra-feature-store
```

```javascript
import { FeatureStore } from 'wickra-feature-store'

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
console.log(matrix.columns)
```

## More

- [npmjs.com/package/wickra-feature-store](https://www.npmjs.com/package/wickra-feature-store)
- [Source & examples](https://github.com/wickra-lib/wickra-feature-store/tree/main/examples/node)
