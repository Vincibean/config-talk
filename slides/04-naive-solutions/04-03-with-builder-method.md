//TODO
```scala
for {
  app <- Play.maybeApplication
  conf <- app.getConfig
  x <- conf.getString("foo")
  y <- conf.getInt("bar")
  ...
} builderMethodWithALotOfParameters(x: String, y: Int, z: File, ...)
```
