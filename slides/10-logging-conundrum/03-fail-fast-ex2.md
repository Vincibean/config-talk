```scala
val optBuilder: Option[SamlBuilder[FullConfig]] = for {
  app <- Play.maybeApplication
  conf = app.configuration
  logoutUrl <- conf.getString("saml.logout-url")
  _ = Logger.debug(s"Logout URL is $logoutUrl")
  keyStorePath <- conf.getString("saml.configs.keystore.path")
  _ = Logger.debug(s"Keystore Path is $keyStorePath")
  idpMetadataPath <- conf.getString("saml.configs.idp-metadata-url")
  _ = Logger.debug(s"IDP metadata path is $idpMetadataPath")
} yield SamlBuilder()
  .withLogoutUrl(logoutUrl)
  .withKeystorePath(keyStorePath)
  .withIdpMetadataPath(idpMetadataPath)
val builder = optBuilder.get
builder.build()
```
