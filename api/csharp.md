# C\#

The .NET binding wraps the C ABI. Construct a `FeatureStore` from a JSON spec and
drive it with `Command(json) -> json`.

```bash
dotnet add package Wickra.FeatureStore
```

```csharp
using Wickra.FeatureStore;

const string spec = """
{"universe":["AAA"],
 "features":[{"kind":"price","field":"close"}],
 "labels":[{"kind":"forward_return","horizon":1}]}
""";

using var store = new FeatureStore(spec);
var matrix = store.Command("""{"cmd":"build_batch","data":{"AAA":[{"ts":0,"open":100,"high":100,"low":100,"close":100,"volume":1}]}}""");
Console.WriteLine(matrix);
```

## More

- [nuget.org/packages/Wickra.FeatureStore](https://www.nuget.org/packages/Wickra.FeatureStore)
- [Source & examples](https://github.com/wickra-lib/wickra-feature-store/tree/main/examples/csharp)
