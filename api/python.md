# Python

The Python package wraps the Rust core over the C ABI. Construct a `FeatureStore`
from a JSON spec and drive it with `command(json) -> json`.

```bash
pip install wickra-feature-store
```

```python
import json
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
matrix = json.loads(store.command(json.dumps({"cmd": "build_batch", "data": data})))
print(matrix["columns"])
```

## More

- [pypi.org/project/wickra-feature-store](https://pypi.org/project/wickra-feature-store/)
- [Source & examples](https://github.com/wickra-lib/wickra-feature-store/tree/main/examples/python)
- [Features & labels](https://github.com/wickra-lib/wickra-feature-store/blob/main/docs/FEATURES.md)
