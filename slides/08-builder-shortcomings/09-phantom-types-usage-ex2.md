```scala
for {
  app <- Play.maybeApplication
  conf = app.configuration
  logoutUrl <- conf.getString("saml.logout-url")
  keyStorePath <- conf.getString("saml.configs.keystore.path")
  idpMetadataPath <- conf.getString("saml.configs.idp-metadata-url")
} {
  SamlBuilder()
    .withLogoutUrl(logoutUrl)
    .withKeystorePath(keyStorePath)
    .withIdpMetadataPath(idpMetadataPath)
    .build()
}
```
