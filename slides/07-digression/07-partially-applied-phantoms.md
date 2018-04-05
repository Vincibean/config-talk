## Consider the similarities
```scala
val sum = (a: Int) => (b: Int) => a + b
val sum1 = sum(1)
val res = sum1(6)
println(res)
```

```scala
class SamlBuilder { ... }
val acsb: SamlBuilder[AlmostFullConfig] = SamlBuilder().withKeystorePath(???).withKeystorePassword(???)...
val csb: SamlBuilder[FullConfig] = acsb.withIdpMetadataPath(???)
csb.build()
```
