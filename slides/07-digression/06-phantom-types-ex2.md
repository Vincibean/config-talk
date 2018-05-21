```scala
object SamlBuilder {
  sealed trait Config
  sealed trait EmptyConfig extends Config
  sealed trait LogoutUrl extends Config
  sealed trait KeystorePath extends Config
  sealed trait IdpMetadataPath extends Config

  type AlmostFullConfig = EmptyConfig with 
                    LogoutUrl with 
                    KeystorePath 

  type FullConfig = AlmostFullConfig with 
                    IdpMetadataPath

  type AlmostCompleteSamlBuilder = SamlBuilder[AlmostFullConfig]
  type CompleteSamlBuilder = SamlBuilder[FullConfig]

  private case class SamlConfigsAccumulator(
    logoutUrl: String = "",
    keyStorePath: String = "",
    idpMetadataPath: String = ""
  )

  private def apply[T <: Config](accumulator: SamlConfigsAccumulator): SamlBuilder[T] = 
    new SamlBuilder[T](accumulator)

  def apply(): SamlBuilder[EmptyConfig] = 
    apply[EmptyConfig](SamlConfigsAccumulator())

}

class SamlBuilder[T <: SamlBuilder.Config] private(acc: SamlConfigsAccumulator) {

  def withKeystorePath(keystorePath: String): SamlBuilder[T with KeystorePath] = 
    SamlBuilder(acc.copy(keyStorePath = keystorePath))

  def withIdpMetadataPath(idpMetadata: String): SamlBuilder[T with IdpMetadataPath] = 
    SamlBuilder(acc.copy(idpMetadataPath = idpMetadata))

  def withLogoutUrl(logoutUrl: String): SamlBuilder[T with LogoutUrl] = 
    SamlBuilder(acc.copy(logoutUrl = logoutUrl))

  def build()(implicit ev: T =:= FullConfig): Unit = {
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
