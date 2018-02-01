## Config? What's The Big Deal?

```scala
import com.typesafe.config.ConfigFactory

val conf = ConfigFactory.load()
val bar1 = conf.getInt("foo.bar")
```
