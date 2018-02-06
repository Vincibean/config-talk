```scala
val optContainer: Option[Container] = for {
  app <- Play.maybeApplication
  conf <- app.getConfig
  ...
} yield Container(logoutUrl, keyStorePath, keyStorePassword, privateKeyPassword, idpMetadataPath, samlClientName, spEntityId, maximumAuthenticationLifetime)
val container = optContainer.get
builderMethod(container)
```
