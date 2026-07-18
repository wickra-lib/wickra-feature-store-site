# Go

The Go binding links the C ABI via cgo. Construct a `FeatureStore` from a JSON spec
and drive it with `Command(json) -> (json, error)`.

```bash
go get github.com/wickra-lib/wickra-feature-store-go
```

```go
package main

import (
	"fmt"

	wickra "github.com/wickra-lib/wickra-feature-store-go"
)

func main() {
	spec := `{"universe":["AAA"],` +
		`"features":[{"kind":"price","field":"close"}],` +
		`"labels":[{"kind":"forward_return","horizon":1}]}`

	store, err := wickra.New(spec)
	if err != nil {
		panic(err)
	}
	defer store.Close()

	resp, err := store.Command(`{"cmd":"build_batch","data":{"AAA":[{"ts":0,"open":100,"high":100,"low":100,"close":100,"volume":1}]}}`)
	if err != nil {
		panic(err)
	}
	fmt.Println(resp)
}
```

## More

- [pkg.go.dev/github.com/wickra-lib/wickra-feature-store-go](https://pkg.go.dev/github.com/wickra-lib/wickra-feature-store-go)
- [Source & examples](https://github.com/wickra-lib/wickra-feature-store/tree/main/examples/go)
