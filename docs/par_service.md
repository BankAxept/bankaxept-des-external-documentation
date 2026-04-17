# PAR Service

The PAR Service is a service provided by BankAxept DES. The intention is to provide a 
PAR (Payment Account Reference) value for integrators and stakeholders in the PAR availability zone.

<p align="center">
<img alt="baxdes_par_availability_zone.png" src="../assets/images/baxdes_par_availability_zone.png" width="1000"/>
</p>

Retailers, PSPs, etc. who own or are a part of a loyalty programme may request PAR for all PANs or Payment Accounts without PAR and store them in their database. 
By ensuring that PAR is also linked to the customer’s account, it is always possible to identify a customer’s account regardless if PAR or PAN is known.

## PAR structures
To this end, EMVCo has specified PAR accordingly:

• A unique non-financial reference/identifier associated with a specific cardholder PAN.

• A 29 character identification number that can be used in place of sensitive consumer identification
fields and transmitted across the payments ecosystem to facilitate consumer identification.

<p align="center">
<img alt="par_structure.png" src="../assets/images/par_structure.png" width="1000"/>
</p>


PAR represents the Payment Account at the same level that PAN represents the Payment Account. It
makes it possible to track and manage accounts across multiple changing tokens without relying on a
PAN.

## PAR exchange methodology

To ensure that PAR or PAN is always available to identify a customer’s payment account, BankAxept re-
quires that PAR is stored in all instruments that utilise tokens, i.e. mobiles, mobile QR Codes, active
wearables, etc. that do not use PANs.

The BankAxept PAR Enquiry Service receives a PAN or a BAN. If a BAN is received, the PAR Enquiry Service converts the BAN to a PAN. 
The PAR Enquiry Service then performs one of the following two actions:

1.	If a PAR exists for the PAN, returns the PAR.
2.	If a PAR does not exist for the PAN, generates a PAR, links it to the PAN and returns the PAR.

## Configuration for service access

The following configuration is required to access the PAR Service:

1. A certificate exchange must performed and mTLS must be enabled for all endpoints. The TLS client certificate must be signed by the BankAxept DES CA.
2.  A key exchange need to be performed to for the encryption and decryption of the PAR data. The integrator must send a public key to BankAxept, which will be used to encrypt the PAR data. The integrator will then be able to decrypt the PAR data using their private key. 
3. A key exchange for the opposite direction must also be performed for the encryption of PAN/BAN data in the request. The integrator will encrypt the PAN/BAN data using a public key provided by BankAxept, and BankAxept will decrypt the data using the corresponding private key.
4. IPs need to be whitelisted for production access.

### Configuration items

#### Information from the integrator:

| Item                                                    | Comment                                              |
|---------------------------------------------------------|------------------------------------------------------|
| IP                                                      | Only required for production                         |
| TLS client certificate to be signed                     |                                                      |
| Fingerprint of the TLS cert                             |                                                      |
| Requestor ID                                            | Preferably something descriptive and human readable. |
| Key identifier of encryption key to use in the response |                                                      |
| Public key for encrypting PAR data in response          | Must be X509 format.                                 |

#### Information to the integrator

| Item                                              | Comment |
|---------------------------------------------------|---------|
| Signed TLS certificate for client                 |         |
| Public key for encryption of PAN/BAN data in request |      |
| Key identifier for key-pair that will be used in the request | |

## Additional resources:

[EMV® Payment Tokenisation Specification - Technical Framework](https://www.emvco.com/emv-technologies/payment-tokenisation/)

[OpenAPI page](./swagger/par-service-api.md).
