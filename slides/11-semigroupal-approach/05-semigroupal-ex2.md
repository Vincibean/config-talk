```scala
val conf: Option[Configuration] = Play.maybeApplication.map(_.configuration)
val logoutUrl: Option[String] = conf.flatMap(_.getString("saml.logout-url"))
val keyStorePath: Option[String] = conf.flatMap(_.getString("saml.configs.keystore.path"))
val idpMetadataPath: Option[String] = conf.flatMap(_.getString("saml.configs.idp-metadata-url"))
val optBuilder: Option[SamlBuilder[FullConfig]] = 
  (logoutUrl, keyStorePath, idpMetadataPath)
    .mapN((ksPath, ksPsw, pkPsw, idpPath, spId, name, cbUrl, loUrl, lfTime) => SamlBuilder()
      .withLogoutUrl(loUrl)
      .withKeystorePath(ksPath)
      .withIdpMetadataPath(idpPath)
val builder: SamlBuilder[FullConfig] = optBuilder.get
builder.build()
```
