# R

The R package links the C ABI. Build a feature store from a JSON spec with
`wkfeaturestore_new`, then drive it with `wkfeaturestore_command`.

```r
install.packages("wickrafeaturestore", repos = "https://wickra-lib.r-universe.dev")
```

```r
library(wickrafeaturestore)

spec <- '{"universe":["AAA"],
          "features":[{"kind":"price","field":"close"}],
          "labels":[{"kind":"forward_return","horizon":1}]}'

store <- wkfeaturestore_new(spec)
matrix <- wkfeaturestore_command(
  store,
  '{"cmd":"build_batch","data":{"AAA":[{"ts":0,"open":100,"high":100,"low":100,"close":100,"volume":1}]}}'
)
cat(matrix)
```

## More

- [wickra-lib.r-universe.dev](https://wickra-lib.r-universe.dev)
- [Source & examples](https://github.com/wickra-lib/wickra-feature-store/tree/main/examples/r)
