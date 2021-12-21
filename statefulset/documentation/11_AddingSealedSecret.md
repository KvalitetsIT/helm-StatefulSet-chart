## Adding sealed secret
Parameter | Description | Example
--- | --- | ---
**Sealed Secret** |
`sealedSecret.{name}` | Name of secret |
`sealedSecret.{name}.type` | Type of the secret - Default Opaque | `kubernetes.io/tls`
`sealedSecret.{name}.encryptedData` | List of 'Key: Value' pair of the encrypted data | `password: AgBOQOoh7RGqTBPPSG0Ctbf...`

> see full configuration [here]( #Configuration)

With sealed secrets it is possible to store your secrets in github with no security-risk. Sounds like something for you? [Follow this guide!](https://doc.hosting.kitkube.dk/deployment/secrets/)
```yaml
sealedSecret:
  my-secret:
    type: Opaque
    encryptedData:
      password: myEncryptedSecretPassword #remember to encrypt value!
      otherPassword: myOtherEncryptedAndMaybeRedundantPassword #remember to encrypt value!
      thirdPassword: encryptedQuestionWhySoManyPasswords? #remember to encrypt value!
```
