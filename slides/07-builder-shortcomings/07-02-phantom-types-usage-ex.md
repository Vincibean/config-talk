```scala
for {
  app <- Play.maybeApplication
  conf <- app.getConfig
  logoutUrl <- conf.getString("saml.logout-url")
  keyStorePath <- conf.getString("saml.configs.keystore.path")
  keyStorePassword <- conf.getString("saml.configs.keystore.password")
  privateKeyPassword <- conf.getString("saml.configs.keystore.private-key-password")
  idpMetadataPath <- conf.getString("saml.configs.idp-metadata-url")
  samlClientName <- conf.getString("saml.configs.saml-url.enabled-domains.client-name")
  spEntityId <- conf.getString("saml.configs.sp-metadata-dir")
  callbackUrl <- conf.getString("saml.fallback-url")
  logoutUrl <- conf.getString("saml.logout-url")
  maximumAuthenticationLifetime <- conf.underlying.getDuration("saml.configs.max-auth-lifetime", TimeUnit.MILLISECONDS)
} {
  SamlBuilder()
    .withKeystorePath(keystorePath)
    .withKeystorePassword(keystorePassword)
    .withPrivateKeyPassword(privateKeyPassword)
    .withIdpMetadataPath(idpMetadata)
    .withSpEntityId(spEntityId)
    .withSamlClientName(samlClientName)
    .withCallbackUrl(callbackUrl)
    .withLogoutUrl(logoutUrl)
    .withMaximumAuthenticationLifetime(maxAuthLifetime)
    .build()
}
```
