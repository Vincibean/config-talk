```scala
val conf = Play.maybeApplication.map(_.configuration).get
val logoutUrl = conf.getString("saml.logout-url").get
val keyStorePath = conf.getString("saml.configs.keystore.path").get
val idpMetadataPath = conf.getString("saml.configs.idp-metadata-url").get
val builder = SamlBuilder()
  .withLogoutUrl(logoutUrl)
  .withKeystorePath(keyStorePath)
  .withIdpMetadataPath(idpMetadataPath)
builder.build()
```
