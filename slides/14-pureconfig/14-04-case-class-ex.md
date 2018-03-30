```scala
case class Keystore(path: String, password: String, privateKeyPassword: String)

case class EnabledDomain(domain: String, clientName: String)

case class SamlUrl(callback: String, enabledDomains: Set[EnabledDomain])

case class SamlConfig(
	partner: String, 
	keystore: Keystore, 
	samlUrl: SamlUrl, 
	idpMetadataUrl: String, 
	spMetadataDir: String, 
	maxAuthLifetime: FiniteDuration)

case class Saml(configs: Set[SamlConfig], fallbackUrl: String, logoutUrl: String)
```
