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
  maximumAuthenticationLifetime <- conf.underlying.getDuration("saml.configs.max-auth-lifetime", TimeUnit.MILLISECONDS)
} {
    BaseConfig.setDefaultLogoutUrl(logoutUrl)

    val saml2Client = new Saml2Client()
    saml2Client.setKeystorePath(keyStorePath)
    saml2Client.setKeystorePassword(keyStorePassword)
    saml2Client.setPrivateKeyPassword(privateKeyPassword)
    saml2Client.setIdpMetadataPath(idpMetadataPath)
    saml2Client.setName(samlClientName)
    saml2Client.setSpEntityId(spEntityId)
    saml2Client.setMaximumAuthenticationLifetime(maximumAuthenticationLifetime.length.toInt)

    val clients = new Clients(callbackUrl, saml2Client)
    Config.setClients(clients)
}
```
