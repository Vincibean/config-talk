```scala
val optBuilder: Option[SamlBuilder[FullConfig]] = for {
  app <- Play.maybeApplication
  conf = app.configuration
  logoutUrl <- conf.getString("saml.logout-url")
  keyStorePath <- conf.getString("saml.configs.keystore.path")
  idpMetadataPath <- conf.getString("saml.configs.idp-metadata-url")
} yield SamlBuilder()
         .withLogoutUrl(logoutUrl)
         .withKeystorePath(keyStorePath)
         .withIdpMetadataPath(idpMetadataPath)
val builder = optBuilder.get
builder.build()
```
