```scala
object SamlBuilder {
  sealed trait Config
  sealed trait EmptyConfig extends Config
  sealed trait KeystorePath extends Config
  sealed trait KeystorePassword extends Config
  sealed trait PrivateKeyPassword extends Config
  sealed trait IdpMetadataPath extends Config
  sealed trait SpEntityId extends Config
  sealed trait SamlClientName extends Config
  sealed trait CallbackUrl extends Config
  sealed trait LogoutUrl extends Config
  sealed trait MaxAuthLifetime extends Config

  type FullConfig = EmptyConfig with 
                    KeystorePath with 
                    KeystorePassword with 
                    PrivateKeyPassword with 
                    SpEntityId with 
                    SamlClientName with 
                    CallbackUrl with 
                    LogoutUrl with 
                    MaxAuthLifetime with 
                    IdpMetadataPath

  type CompleteSamlBuilder = SamlBuilder[FullConfig]

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

  private def apply[T <: Config](accumulator: SamlConfigsAccumulator): SamlBuilder[T] = 
    new SamlBuilder[T](accumulator)

  def apply(): SamlBuilder[EmptyConfig] = 
    apply[EmptyConfig](SamlConfigsAccumulator())

}

class SamlBuilder[T <: SamlBuilder.Config] private(acc: SamlConfigsAccumulator) {

  def withKeystorePath(keystorePath: String): SamlBuilder[T with KeystorePath] = 
    SamlBuilder(acc.copy(keyStorePath = keystorePath))

  def withKeystorePassword(keystorePassword: String): SamlBuilder[T with KeystorePassword] = 
    SamlBuilder(acc.copy(keyStorePassword = keystorePassword))

  def withPrivateKeyPassword(privateKeyPassword: String): SamlBuilder[T with PrivateKeyPassword] = 
    SamlBuilder(acc.copy(privateKeyPassword = privateKeyPassword))

  def withIdpMetadataPath(idpMetadata: String): SamlBuilder[T with IdpMetadataPath] = 
    SamlBuilder(acc.copy(idpMetadataPath = idpMetadata))

  def withSpEntityId(spEntityId: String): SamlBuilder[T with SpEntityId] = 
    SamlBuilder(acc.copy(spEntityId = spEntityId))

  def withSamlClientName(samlClientName: String): SamlBuilder[T with SamlClientName] = 
    SamlBuilder(acc.copy(samlClientName = samlClientName))

  def withCallbackUrl(callbackUrl: String): SamlBuilder[T with CallbackUrl] = 
    SamlBuilder(acc.copy(callbackUrl = callbackUrl))

  def withLogoutUrl(logoutUrl: String): SamlBuilder[T with LogoutUrl] = 
    SamlBuilder(acc.copy(logoutUrl = logoutUrl))

  def withMaximumAuthenticationLifetime(maxAuthLifetime: FiniteDuration): SamlBuilder[T with MaxAuthLifetime] = 
    SamlBuilder(acc.copy(maximumAuthenticationLifetime = maxAuthLifetime))

  def build(implicit ev: T =:= FullConfig, app: Application): Unit = {
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
