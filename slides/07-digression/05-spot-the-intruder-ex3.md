## And Here?
```
saml {
  configs = [
    {
      partner = "partner1"
      keystore {
        path = "path/to/samlKeystore.jks"
        password = "java-keystore-password"
        private-key-password = "certificate-password"
      }
      saml-url {
        callback = "/auth/saml/callback"
        enabled-domains = [
          { domain = "domain01:9443", client-name = "samlAccount" }
        ]
      }
      idp-metadata-url = "https://my.identiy-provider.com/FederationMetadata/2017-06/FederationMetadata.xml"
      sp-metadata-dir = "./target"
      max-auth-lifetime = 8 hours
    }
  ]
}
```
