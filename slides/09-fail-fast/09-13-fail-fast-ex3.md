```scala
val conf = Play.maybeApplication.map(_.configuration).get
val logoutUrl = conf.getString("saml.logout-url").get
val keyStorePath = conf.getString("saml.configs.keystore.path").get
val keyStorePassword = conf.getString("saml.configs.keystore.password").get
val privateKeyPassword = conf.getString("saml.configs.keystore.private-key-password").get
val idpMetadataPath = conf.getString("saml.configs.idp-metadata-url").get
val samlClientName = conf.getString("saml.configs.saml-url.enabled-domains.client-name").get
val spEntityId = conf.getString("saml.configs.sp-metadata-dir").get
val callbackUrl = conf.getString("saml.fallback-url").get
val maxAuthLifetime = conf.underlying.getDuration("saml.configs.max-auth-lifetime", TimeUnit.MILLISECONDS).millis
val builder = SamlBuilder()
  .withKeystorePath(keyStorePath)
  .withKeystorePassword(keyStorePassword)
  .withPrivateKeyPassword(privateKeyPassword)
  .withIdpMetadataPath(idpMetadataPath)
  .withSpEntityId(spEntityId)
  .withSamlClientName(samlClientName)
  .withCallbackUrl(callbackUrl)
  .withLogoutUrl(logoutUrl)
  .withMaximumAuthenticationLifetime(maxAuthLifetime)
builder.build()
```
