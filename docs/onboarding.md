# Onboarding

BankAxept offers a range of services to Issuers and Merchants. This document outlines the onboarding process for these services.

## xPays

In order to support xPays with BankAxept a technical configuration is required.
This includes setting up your Issuing Bank values in BankAxept, and configuring your system in our TSH OEM broker (Thales).

### Steps to activate

A series of steps and tests needs to be completed by multiple participants in order ensure high quality and compliance.

| Step                                   | Explanation                                                                                                             | Participants                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| Onboarding registration                | Configure profile values as defined in our [Checklist section](#checklist)                                              | Issuing Bank (Responsible), BankAxept, Issuing Processor       |
| Onboarding Thales                      | Setup bank in Thales Portal. Ordered by BankAxept Command Center.                                                       | BankAxept (Responsible), Thales                                |
| Testing                                | Agree upon Whitelisted Pilot cards. **Remember** to consider PCI compliance during discussion.                          | Bank(Responsible), BankAxept, TietoEvry                        |
| Key Management                         | Exchange keys.                                                                                                          | BankAxept(Responsible), Thales                                 |
| Visa Environment                       | Configure VISA values in Prod environment.                                                                              | Bank(Responsible), VISA                                        |
| Transaction Verification               | Agreement with VISA - change to right PCR.                                                                              | Bank (Responsible), VISA, TietoEvry/Nexi                       |
| Transaction Verification               | Replace URL in VTS.                                                                                                     | Bank(Responsible), VISA                                        |
| Transaction Verification               | Dev for Select BankAxept transactions  and Visa.                                                                        | Issuer Processor (Responsible), Bank                           |
| Transaction Verification               | Establish connectivity to Thales.                                                                                       | BankAxept (Responsible), Thales, Issuer Processor, Bank        |
| Transaction Verification               | Testing TranId API.                                                                                                     | BankAxept (Responsible), Thales, Issuer Processor, Bank, Apple |
| Migrating Cards                        | Not Applicable for all scenarios. If you wish to migrate cards you must create a list of cards and send them to Thales. | Bank(Responsible), Thales, Issuer Processor, BankAxept, Apple  |
| Certification                          | Certification setup, including payment.                                                                                 | Bank(Responsible), Apple, Fime(?), BankAxept                   |
| Non functional Requirements from Apple | Not applicable for all scenarios. Only if it affects BankAxept.                                                         | Bank(Responsible), BankAxept, Apple                            |
| Call Center                            | Onboard call centre.                                                                                                    | Bank(Responsible)                                              |
| BITS-Certification                     |                                                                                                                         | BankAxept(Responsible), Nets, Bits                             |
| VISA Environment                       | VISA VTS Setup.                                                                                                         | Bank(Responsible), VISA                                        |
| Launch Day                             | Availability and testing on launch day.                                                                                 | Bank(Responsible), Apple                                       |

### Setting up Mutual TLS

Mutual TLS must secure all communication.
Only TLS1.2 or higher is supported.
A secure exchange of certificates must be performed before any
communication can be done.

### Checklist

The following data points and checks are required to be set up in the onboarding process in order to support Apple Pay.

| Step                     | Explanation                                                                       |
|--------------------------|-----------------------------------------------------------------------------------|
| Registration data points | Regnumber, Issuer ID and applicable VISA BIN ranges must be sent for registration |
| Pilot cards              | Bank needs to coordinate which cards should be whitelisted for testing            |
