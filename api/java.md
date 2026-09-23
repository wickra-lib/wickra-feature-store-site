# Java

The Java binding links the C ABI via a small JNI shim. Construct a `FeatureStore`
from a JSON spec and drive it with `command(json) -> json`.

```xml
<!-- Maven Central -->
<dependency>
  <groupId>org.wickra</groupId>
  <artifactId>wickra-feature-store</artifactId>
  <version>0.1.3</version>
</dependency>
```

```java
import org.wickra.featurestore.FeatureStore;

String spec = """
    {"universe":["AAA"],
     "features":[{"kind":"price","field":"close"}],
     "labels":[{"kind":"forward_return","horizon":1}]}
    """;

try (FeatureStore store = new FeatureStore(spec)) {
    String matrix = store.command(
        "{\"cmd\":\"build_batch\",\"data\":{\"AAA\":[{\"ts\":0,\"open\":100,\"high\":100,\"low\":100,\"close\":100,\"volume\":1}]}}");
    System.out.println(matrix);
}
```

## More

- [central.sonatype.com/artifact/org.wickra/wickra-feature-store](https://central.sonatype.com/artifact/org.wickra/wickra-feature-store)
- [Source & examples](https://github.com/wickra-lib/wickra-feature-store/tree/main/examples/java)
