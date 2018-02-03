```scala
import com.typesafe.config.ConfigFactory

val conf = ConfigFactory.load()
val bar = conf.getInt("foo.bar")
```
