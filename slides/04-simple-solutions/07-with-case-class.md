```scala
for {
  app <- Play.maybeApplication
  conf = app.configuration
  logoutUrl <- conf.getString("saml.logout-url")
  keyStorePath <- conf.getString("saml.configs.keystore.path")
  idpMetadataPath <- conf.getString("saml.configs.idp-metadata-url")
  container = Container(logoutUrl, keyStorePath, idpMetadataPath)
} builderMethod(container)
```
