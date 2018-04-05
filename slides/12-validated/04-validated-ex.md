```scala
import cats.Semigroupal
import cats.data.{NonEmptyVector, Validated}

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

val valBuilder: Validated[NonEmptyVector[String], SamlBuilder[FullConfig]] = Semigroupal.map9(
  Validated.fromOption[NonEmptyVector[String], String](logoutUrl, NonEmptyVector.of("Missing saml.logout-url")),
  Validated.fromOption[NonEmptyVector[String], String](keyStorePath, NonEmptyVector.of("Missing saml.configs.keystore.path")),
  Validated.fromOption[NonEmptyVector[String], String](keyStorePassword, NonEmptyVector.of("Missing saml.configs.keystore.password")),
  Validated.fromOption[NonEmptyVector[String], String](privateKeyPassword, NonEmptyVector.of("Missing saml.configs.keystore.private-key-password")),
  Validated.fromOption[NonEmptyVector[String], String](idpMetadataPath, NonEmptyVector.of("Missing saml.configs.idp-metadata-url")),
  Validated.fromOption[NonEmptyVector[String], String](samlClientName, NonEmptyVector.of("Missing saml.configs.saml-url.enabled-domains.client-name")),
  Validated.fromOption[NonEmptyVector[String], String](spEntityId, NonEmptyVector.of("Missing saml.configs.sp-metadata-dir")),
  Validated.fromOption[NonEmptyVector[String], String](callbackUrl, NonEmptyVector.of("Missing saml.fallback-url")),
  Validated.fromOption[NonEmptyVector[String], FiniteDuration](maxAuthLifetime, NonEmptyVector.of("Missing saml.configs.max-auth-lifetime"))
)((ksPath, ksPsw, pkPsw, idpPath, spId, name, cbUrl, loUrl, lfTime) => SamlBuilder()
  .withKeystorePath(ksPath)
  .withKeystorePassword(ksPsw)
  .withPrivateKeyPassword(pkPsw)
  .withIdpMetadataPath(idpPath)
  .withSpEntityId(spId)
  .withSamlClientName(name)
  .withCallbackUrl(cbUrl)
  .withLogoutUrl(loUrl)
  .withMaximumAuthenticationLifetime(lfTime))

val eBuilder = valBuilder.toEither
eBuilder.left.foreach(nev => Logger.error(nev.toVector.mkString(", ")))

val builder: SamlBuilder[FullConfig] = eBuilder.right.get
builder.build()
```
