# Tokenization

## Introduction

DES provides digitization, tokenization, provisioning and lifecycle management of payment credentials.
This is being performed on behalf of Norwegian issuers and is achieved through a connection with a Baltus Node.

This documentation outlines the interplay between the DES Issuer API and the Issuer Processor. It does not go into depth on the ecosystem outside this
relationship.

### xPays

DES may be utilized for tokenization for various xPays. This enables BankAxept to tokenize on behalf of the BankAxept Issuing Banks.
Below is an illustration representing the ecosystem in which DES operates.

<p align="center">
<img alt="Baxdes_flow_xpays.png" src="../assets/images/baxdes_flow_xpays.png" width="1000"/>
</p>

### HCE wallets

DES may be utilized for tokenization for HCE wallets. This enables BankAxept to tokenize on behalf of the BankAxept Issuing Banks.
Below is an illustration representing the ecosystem in which DES operates.

<p align="center">
<img alt="Baxdes_flow_hce.png" src="../assets/images/baxdes_flow_hce.png" width="1000"/>
</p>

## Tokenization flow

## Full overview

This is intended as an overview of the tokenization process in the ecosystem in a co-badged setting.
Very rarely does a single actor need to know the full process.
This is intended more of a reference for understanding the full process.
Please peruse our [dictionary](./dictionary.md) for term explanation.

BankAxept's role can be seen in the Auxiliary token provisioning step, note that for epp and mono-badged it is performed as seen in
the [EPP flow](#epp-flow). The IGW (Issuer Gateway) is DES's service for interfacing with the Issuer Service.

```mermaid
sequenceDiagram
    autonumber
    title Scandium Provisioning Flow Green
    box rgb(204, 204, 255) Client Device
        participant wallet as Wallet
    end
    box rgb(170, 221, 238) "Apple Server"
        participant broker as SMP-Broker
        participant tsm as SMP-TSM
    end
    box rgb(255, 170, 170) "Primary Partner"
        participant primaryBroker as ICS Broker
        participant primaryTSM as ICS TSM
        participant primaryTSP as ICS TSP
    end
    box rgb(255, 170, 202) "Auxiliary Partner"
        participant auxBroker as Thales Broker
        participant auxTSM as Thales TSM
        participant auxTSP as BankAxept TSP
        participant auxIGW as BankAxept IGW
    end
    box rgb(188, 255, 194) "Issuer"
        participant issuerGtw as Issuer Service
        participant issuer as Issuing Bank
    end

    note over wallet: User opens Wallet and taps<br/> + selects provisioning option and <br/> enters basic card data via camera or manual entry
    Note over wallet, issuer: Eligibility Check
    wallet ->> broker: Eligibility Check
    broker ->> primaryBroker: NetworkCheckCard
    primaryBroker ->> issuerGtw: VerifyAccount
    issuerGtw ->> issuer: Eligibility Check
    Note over issuer: Account status check
    issuer -->> issuerGtw: EligibilityCheck response
    issuerGtw -->> primaryBroker: VerifyAccount response
    primaryBroker -->> broker: NetworkCheckCard response
    broker -->> wallet: EligibilityCheck response
    Note over wallet, issuer: Primary Token Provisioning
    wallet ->> broker: EnableCard
    broker ->> primaryBroker: Link&Provision
    primaryBroker ->> issuerGtw: AuthoriseProvisioning(green recommendation)
    issuerGtw ->> issuer: Approve Provisioning
    Note over issuer: Fraud processing
    issuer -->> issuerGtw: Approve Provisioning Response
    issuerGtw -->> primaryBroker: AuthoriseProvisioning(green) Response
    Note over primaryTSM: Internal create token and person requests
    primaryBroker -->> broker: Link&Provision response
    broker -->> wallet: EnableCard response
    Note over wallet: Primary Token <br/> inactive in Wallet
    Note over wallet: Primary partner then<br/> delivers and applies <br/> perso script to SE
    Note over wallet, issuer: Auxiliary Token Provisioning
    broker ->> auxBroker: createToken
    auxBroker ->> auxIGW: requestCoBadgeDigitization <br/>- encrypted VISA
    auxIGW ->> issuerGtw: requestCoBadgeDigitization <br/>- encrypted VISA
    issuerGtw -->> auxIGW: - encryptedCardInfo <br/>- encrypted BankAxept
    auxIGW -->> auxBroker: requestCoBadgeDigitization Response <br/>- BankAxept PAN++
    auxBroker ->> auxTSP: createToken
    auxTSP -->> auxBroker: createToken Response
    auxBroker -->> broker: createToken Response
    auxTSP ->> auxBroker: submitTokenData
    auxBroker -->> auxTSP: submitTokenData Response
    Note over auxBroker: Create perso script
    broker ->> tsm: activateSecondaryToken
    Note over auxBroker: Handle perso script
    auxBroker ->> auxTSP: updateTokenState <br/>- ACTIVE
    Note over auxTSP: Activate token
    auxTSP -->> auxBroker: updateTokenState Response
    auxBroker ->> auxIGW: notifyVirtualCardChange
    auxIGW ->> issuerGtw: digitizationCompleteNotification
    issuerGtw -->> auxIGW: digitizationCompleteNotification Response
    auxIGW -->> auxBroker: notifyVirtualCardChange Response
    Note over wallet, issuer: Activation Steps
    tsm ->> wallet: Deliver perso script
    Note over wallet: Applies Auxiliary perso script to SE
    wallet ->> tsm: Return perso script results
    primaryBroker ->> issuerGtw: notify provisioning result
    issuerGtw -->> primaryBroker: Request
    issuerGtw ->> issuer: notify provisioning result
    issuer -->> issuerGtw: Request
    Note over broker: Update pass details
    broker ->> wallet: Activate Pass
    Note over wallet: Pass Active


```

### xPay Flow

See the dedicated section for more information [here](./enrolment_xpays.md).
Once an entity(xPay etc.) has started a tokenization process, the following flow is expected between the Issuer Processor and DES.

```mermaid

sequenceDiagram
    participant DES
    participant IssuerService
    DES ->> IssuerService: RequestCardDigitization
    IssuerService -->> DES: CardDigitizationResponse
    DES ->> IssuerService: DigitizationCompleteNotification
    IssuerService -->> DES: DigitizationCompleteNotificationResponse
```

### EPP flow

See the dedicated section for more information [here](./enrolment_epayment.md).
Once an entity(xPay etc.) has started a tokenization process, the following flow
is expected between the Issuer Processor and DES.

```mermaid

sequenceDiagram
    participant DES
    participant IssuerService
    DES ->> IssuerService: RequestEligibleCard
    IssuerService -->> DES: EligibleCardResponse
    DES ->> IssuerService: RequestCardDigitization
    IssuerService -->> DES: CardDigitizationResponse
    DES ->> IssuerService: DigitizationCompleteNotification
    IssuerService -->> DES: DigitizationCompleteNotificationResponse
```
