# Rust

The native crate. Fold a `FeatureSpec` over a universe with `build`, or drive a
`FeatureStore` handle with the JSON command protocol every other binding uses.

```bash
cargo add wickra-feature-store
```

```rust
use feature_store_core::{build, Candle, FeatureSpec};
use std::collections::BTreeMap;

let spec = FeatureSpec::from_json(SPEC).expect("valid spec");

let mut data: BTreeMap<String, Vec<Candle>> = BTreeMap::new();
// ... fill `data` with each symbol's candle history ...

let matrix = build(&data, &spec).expect("build");
println!("columns: {:?}", matrix.columns());
```

## More

- [crates.io/crates/wickra-feature-store](https://crates.io/crates/wickra-feature-store) · [docs.rs](https://docs.rs/wickra-feature-store)
- [Source & examples](https://github.com/wickra-lib/wickra-feature-store/tree/main/examples/rust)
- [Features & labels](https://github.com/wickra-lib/wickra-feature-store/blob/main/docs/FEATURES.md) · [Output formats](https://github.com/wickra-lib/wickra-feature-store/blob/main/docs/OUTPUT_FORMATS.md)
