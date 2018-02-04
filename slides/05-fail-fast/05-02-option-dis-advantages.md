## Options
- are great for sequencing computation
  - but we aren't sequencing anything here
- are great when we have a default value 
  - but there's no default value at our disposal here

```scala
val t = Some("SoyaWannaBurger")
t.filter(_.length > 0)
 .map(_.toUpperCase)
 .getOrElse("I'm not hungry")

```
