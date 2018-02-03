//TODO
```scala
for {
  app <- Play.maybeApplication
  conf <- app.getConfig
  x <- conf.getString("foo")
  y <- conf.getInt("bar")
  ...
} {
  val builder = ???
  builder.setX(x)
  builder.setY(y)
  ...
  builder.build()
}
```
