```scala
import play.api.Play

val foo: Option[String] = for {
  app <- Play.maybeApplication      // Option[Application]
  config = app.configuration        // Configuration
  f <- config.getString("bar.foo")  // Option[String]
} yield f
```
