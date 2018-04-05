```scala
object SamlBuilder {
  private case class SamlConfigsAccumulator(
    spEntityId: String = "",
    keyStorePath: String = "",
    keyStorePassword: String = "",
    privateKeyPassword: String = "",
    idpMetadataPath: String = "",
    samlClientName: String = "",
    callbackUrl: String = "",
    maximumAuthenticationLifetime: FiniteDuration = 60.seconds,
    logoutUrl: String = ""
  )

  private def apply(accumulator: SamlConfigsAccumulator): SamlBuilder = 
    new SamlBuilder(accumulator)

  def apply(): SamlBuilder = 
    apply(SamlConfigsAccumulator())

}

class SamlBuilder private(acc: SamlConfigsAccumulator) {

  def withKeystorePath(keystorePath: String): SamlBuilder = 
    SamlBuilder(acc.copy(keyStorePath = keystorePath))

  def withKeystorePassword(keystorePassword: String): SamlBuilder = 
    SamlBuilder(acc.copy(keyStorePassword = keystorePassword))

  def withPrivateKeyPassword(privateKeyPassword: String): SamlBuilder = 
    SamlBuilder(acc.copy(privateKeyPassword = privateKeyPassword))

  def withIdpMetadataPath(idpMetadata: String): SamlBuilder = 
    SamlBuilder(acc.copy(idpMetadataPath = idpMetadata))

  def withSpEntityId(spEntityId: String): SamlBuilder = 
    SamlBuilder(acc.copy(spEntityId = spEntityId))

  def withSamlClientName(samlClientName: String): SamlBuilder = 
    SamlBuilder(acc.copy(samlClientName = samlClientName))

  def withCallbackUrl(callbackUrl: String): SamlBuilder = 
    SamlBuilder(acc.copy(callbackUrl = callbackUrl))

  def withLogoutUrl(logoutUrl: String): SamlBuilder = 
    SamlBuilder(acc.copy(logoutUrl = logoutUrl))

  def withMaximumAuthenticationLifetime(maxAuthLifetime: FiniteDuration): SamlBuilder = 
    SamlBuilder(acc.copy(maximumAuthenticationLifetime = maxAuthLifetime))

  def build: Unit = {
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
