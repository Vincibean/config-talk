## Let's try with a case class

```scala
for {
  app <- Play.maybeApplication
  conf <- app.getConfig
  x <- conf.getString("foo")
  y <- conf.getInt("bar")
  ...
  container = Container(x, y, z, ...)
} builderMethod(container)
```
