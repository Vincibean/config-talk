```scala
object SamlBuilder {
  private case class SamlConfigsAccumulator(
    logoutUrl: String = "",
    keyStorePath: String = "",
    idpMetadataPath: String = ""
  )

  private def apply(accumulator: SamlConfigsAccumulator): SamlBuilder = 
    new SamlBuilder(accumulator)

  def apply(): SamlBuilder = 
    apply(SamlConfigsAccumulator())

}

class SamlBuilder private(acc: SamlConfigsAccumulator) {

  def withKeystorePath(keystorePath: String): SamlBuilder = 
    SamlBuilder(acc.copy(keyStorePath = keystorePath))

  def withIdpMetadataPath(idpMetadata: String): SamlBuilder = 
    SamlBuilder(acc.copy(idpMetadataPath = idpMetadata))

  def withLogoutUrl(logoutUrl: String): SamlBuilder = 
    SamlBuilder(acc.copy(logoutUrl = logoutUrl))

  def build(): Unit = {
    BaseConfig.setDefaultLogoutUrl(acc.logoutUrl)

    val saml2Client = new Saml2Client()
    saml2Client.setKeystorePath(acc.keyStorePath)
    saml2Client.setKeystorePassword(acc.keyStorePassword)
    saml2Client.setPrivateKeyPassword(acc.privateKeyPassword)
    saml2Client.setIdpMetadataPath(acc.idpMetadataPath)
    saml2Client.setName(acc.samlClientName)
    saml2Client.setSpEntityId(acc.spEntityId)
    saml2Client.setMaximumAuthenticationLifetime(acc.maximumAuthenticationLifetime.length.toInt)

    val clients = new Clients(acc.callbackUrl, saml2Client)
    Config.setClients(clients)
  }

}
```
