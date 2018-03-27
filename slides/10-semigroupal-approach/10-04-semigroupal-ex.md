```scala
val conf: Option[Configuration] = Play.maybeApplication.map(_.configuration)
val logoutUrl: Option[String] = conf.flatMap(_.getString("saml.logout-url"))
val keyStorePath: Option[String] = conf.flatMap(_.getString("saml.configs.keystore.path"))
val keyStorePassword: Option[String] = conf.flatMap(_.getString("saml.configs.keystore.password"))
val privateKeyPassword: Option[String] = conf.flatMap(_.getString("saml.configs.keystore.private-key-password"))
val idpMetadataPath: Option[String] = conf.flatMap(_.getString("saml.configs.idp-metadata-url"))
val samlClientName: Option[String] = conf.flatMap(_.getString("saml.configs.saml-url.enabled-domains.client-name"))
val spEntityId: Option[String] = conf.flatMap(_.getString("saml.configs.sp-metadata-dir"))
val callbackUrl: Option[String] = conf.flatMap(_.getString("saml.fallback-url"))
val maxAuthLifetime: Option[FiniteDuration] = conf.map(_.underlying.getDuration("saml.configs.max-auth-lifetime", TimeUnit.MILLISECONDS).millis)
val optBuilder: Option[SamlBuilder[FullConfig]] = Semigroupal.map9(
  keyStorePath,
  keyStorePassword,
  privateKeyPassword,
  idpMetadataPath,
  spEntityId,
  samlClientName,
  callbackUrl,
  logoutUrl,
  maxAuthLifetime)((ksPath, ksPsw, pkPsw, idpPath, spId, name, cbUrl, loUrl, lfTime) => SamlBuilder()
  .withKeystorePath(ksPath)
  .withKeystorePassword(ksPsw)
  .withPrivateKeyPassword(pkPsw)
  .withIdpMetadataPath(idpPath)
  .withSpEntityId(spId)
  .withSamlClientName(name)
  .withCallbackUrl(cbUrl)
  .withLogoutUrl(loUrl)
  .withMaximumAuthenticationLifetime(lfTime))
val builder = optBuilder.get
builder.build()
```
