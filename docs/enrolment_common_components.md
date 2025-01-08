# Card info

The EncryptedCardInfo is a JSON object containing the following fields; it must be sent as a Base64-encoded string of encrypted card information

| Parameter | Description                                                                                                                             | Type     |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------|----------|
| cvv (C)   | Depending on the wallet provider this value may or may not be provided. In the case of InApp enrolment, this data field is not present. | String-3 |
| exp       | The expiry date in the format MMYY                                                                                                      | String-3 |
| fpan      | The funding PAN to digitise                                                                                                             | String-3 |
| psn (C)   | The Pan Sequence Number for the funding PAN to digitise                                                                                 | String-3 |

### Card info encryption guidelines

The cardInfo JSON structure is encrypted then Base64-encoded and placed in the field encryptedCardInfo.
If the encryptedCardInfo is present, the encryption mechanism used is based on the Cryptographic Message.

Syntax as specified in RFC 5652 and using the following parameters:

- The content encryption algorithm used is AES256/CBC/PKCS7 Padding.
- The key encryption algorithm is RSA with a private/public key length of 2048 bits.

The Issuer must provide a public key certificate to be used by the DES to encrypt the symmetric keys used to protect the data fields included in the
encryptedCardInfo field. The Issuer will receive the
certificate to be used when sending encryptedCardInfo to BankAxept.

BankAxept does not impose any requirements on how the issuer protects the corresponding
private key,e.g. protected in an HSM, a hardware token/smart card or in a key file.

As per EMVCo specifications, the psn is a two-digit number, but per ISO
standards the psn is three digits. To accommodate this, the length has been set to up three digits.
