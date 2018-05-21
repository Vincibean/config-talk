```scala
import cats.Semigroupal
import cats.data.{NonEmptyVector, Validated}

val conf: Option[Configuration] = Play.maybeApplication.map(_.configuration)
val logoutUrl: Option[String] = conf.flatMap(_.getString("saml.logout-url"))
val keyStorePath: Option[String] = conf.flatMap(_.getString("saml.configs.keystore.path"))
val idpMetadataPath: Option[String] = conf.flatMap(_.getString("saml.configs.idp-metadata-url"))

val valBuilder: Validated[NonEmptyVector[String], SamlBuilder[FullConfig]] = (
  Validated.fromOption[NonEmptyVector[String], String](logoutUrl, NonEmptyVector.of("Missing saml.logout-url")),
  Validated.fromOption[NonEmptyVector[String], String](keyStorePath, NonEmptyVector.of("Missing saml.configs.keystore.path")),
  Validated.fromOption[NonEmptyVector[String], String](idpMetadataPath, NonEmptyVector.of("Missing saml.configs.idp-metadata-url"))
).mapN((loUrl, ksPath, idpPath) => SamlBuilder()
  .withLogoutUrl(loUrl)
  .withKeystorePath(ksPath)
  .withIdpMetadataPath(idpPath)
)

val eBuilder = valBuilder.toEither
eBuilder.left.foreach(nev => Logger.error(nev.toVector.mkString(", ")))

val builder: SamlBuilder[FullConfig] = eBuilder.right.get
builder.build()
```
