# Self-hosted Mail Server

## Overview

This document provides an analysis of open source SMTP services and an implementation strategy for implementing similar services with a web3 backend.

The self hosted mail server is the foundational component for Email Alias Service V2. It's role is to support SMTP administration and handle the forwarding and managment of emails.

An evaluation of open source SMTP servers can be found in [Appendix A -SMTP Server Review](#appendix-a---smtp-server-review).

## Scenario Overview

Following is an example scenario from [Email Alias Service](#appendix-d---email-alias-service):

> 1. Buy a .country domain (e.g., fsociety.country)
> 2. Activate Email Alias Service by setting a forwarding email address (e.g., mr.robot@gmail.com)
> 3. Done! Now you can receive emails at hello@fsociety.country. All emails will be secretly forwarded to mr.robot@gmail.com

Here is the same example broken down in to the technical components, in this example we assume the use of Maddy email server[^71]

 *Note: Maddy was chosen over mailcow due to it's modular framework [^72] support allowing for development of web3 plugins.*

## Technical Process Flow (High Level)

1. Buy a .country domain (e.g., fsociety.country)
   1. User interacts with dot-country fronted [^5]
   2. Pricing is retrieved from [DC.sol](https://github.com/polymorpher/dot-country/tree/main/contracts/contracts) via the relayer
   3. User Authorizes and purchases the domain using the 1ns contracts deployed via ens-deployer [^9].
2. Activate Email Alias Service by setting a forwarding email address (e.g., mr.robot@gmail.com)
   1. User interacts with EAS Frontend[^6]
   2. EAS Frontend calls interacts directly with `EAS.sol` checking the signer of the alias update transaction is the owner of the domain and updating alias information in EASConfig . *Note: An alternate approach is the front-end calls a DEES API which interacts with the DEES Maddy sender /recipient rewriting(DMSR) custom developed module [^74] or alternatively interacts with the server EAS server[^6].*
   3. If using a go-enabled api server the MSR Module interacts with go-1ns [^10] to update the EAS Contract[^6] setting alias information in the Web3 Backend.
3. Done! Now you can receive emails at hello@fsociety.country. All emails will be secretly forwarded to mr.robot@gmail.com6
   1. An email is sent to hello@fsociety.country.
   2. DEES SMTP Server Receives acts as the MX server to forward the email and processes it via the SMTP Pipeline [^73] which updates the recipient via DEES Maddy sender /recepient rewriting(DMSR) custom developed module [^74] and then forwards the email.

## Infrastructure Components and Development Tasks

*Note for the DEES API development we can either enhance the existing Email Alias Service [^6] function to add SMTP administration API's or build a standlone server in go (Option 2). Initial thoughts is option 1 may be the preferred approach however both options have been included below for completeness.*

1. Email Alias Service [^6]
   1. Server: Modify to use DEES API instead of ImprovX
2. ENS-Deployer [^9]
   1. Include Deployment of EAS.sol (optional)
3. go-1ns [^10]
   1. Add EAS.sol abi
   2. Add read methods for Email Alias Services
   3. *Add update methods for Email Alias Service (Option 2 only)*
4. DEES - MSR Plugin (New Development)
   1. Integrate with go-1ns
   2. Add read functionality for Email Alias Service
5. DEES - API SMTP Administration
   1. Email Alias Service - Server [^6] (Option 1)
      1. Develop [API Layer for SMTP administation](#api-layer-smtp-administation)
   2. DEES API - (Option 2)
      1. Build lightweight go server
      2. Integrate with go-1ns
      3. Develop [API Layer for SMTP administation](#api-layer-smtp-administation)

## Deployment Tasks

1. Deploy DEES Server - leveraging ns1.hiddenstate.xyz (web2 backend) or ns3.hiddenstate.xyz (web3 backend)

## Web3 Plugin for SMTP administration

DEES SMTP Server Receives the email and processes it via the SMTP Pipeline [^73] which updates the recipient via DEES Maddy sender /recepient rewriting(DMSR) custom developed module [^74] and then forwards the email

### Infrastructure Components and Development Tasks**

DEES - MSR Plugin (New Development)

1. Integrate with go-1ns
2. Add read functionality for Email Alias Service

## API Layer (SMTP administation)

The DEES mail server provides the following functionality (exposed by API's[^61] and backed by a web3 backend using IPFS for storage):

1. Account Management: provides the ability to retrieve your DEES account information and a list of whitelabeled domains.
2. Domain Managment: provides the ability to manage domains (list, add, get, update, delete) and check mx entries for the domain.
3. Aliases: provides the ability to manage aliases for a given domain (list, add, bulk add, update, delete).
4. Logs: Provides the ability to retrieve logs for both domains and aliases.
5. SMTP Credentials:  provides the ability to manage SMTP credentials (list, add, update, delete).

For a comparison of SMTP servers see [Appendix A - SMTP Server Review](#appendix-a---smtp-server-review) and for build out approach see [Appendix B - Implementation Strategy](#appendix-b---implementation-strategy).

### Infrastructure Components and Development Tasks**

*Note for the DEES API development we can either enhance the existing Email Alias Service [^6] function to add SMTP administration API's or build a standlone server in go (Option 2). Initial thoughts is option 1 may be the preferred approach however both options have been included below for completeness.*

DEES - API SMTP Administration

1. Email Alias Service - Server [^6] (Option 1)
1. Develop [API Layer for SMTP administation](#api-layer-smtp-administation)
2. DEES API - (Option 2)
1. Build lightweight go server
2. Integrate with go-1ns
3. Develop [API Layer for SMTP administation](#api-layer-smtp-administation)

## Appendices

### Appendix A - SMTP Server Review

Following is an overview of some [open source smtp github repositories](https://github.com/search?o=desc&q=smtp&s=forks&type=Repositories) selection criteria include

1. Language (Go, Node are preferred)
2. Adoption (Number of forks and quality of documentation)
3. Plugin capability (ability to add a plugin or modify the code modularly to integrate with web3 backend and optionally decentralized storage)
4. Extensibility capabilities to extend an API layer to support SMTP administration

Sample Repositories

* [GoLang](https://go.dev/#) [search](https://github.com/search?l=Go&o=desc&q=smtp&s=forks&type=Repositories)
  * [Maddy Mail Server](https://github.com/foxcpp/maddy): Maddy Mail Server implements all functionality required to run a e-mail server. It supports [modules](https://maddy.email/reference/modules/) this is the [Issue capturing Modular Design](https://github.com/foxcpp/maddy/issues/15) and [here are the go docs](https://pkg.go.dev/github.com/foxcpp/maddy@v0.6.2#section-readme). Here is an article[Self-Hosted Email With maddy: A Naive First Attempt](https://mediocregopher.com/posts/selfhosted-email-with-maddy).
  * [go-smtp](https://github.com/emersion/go-smtp)
  * [mail-cow](https://github.com/mailcow/mailcow-dockerized): The integrated mailcow UI allows administrative work on your mail server instance as well as separated domain administrator and mailbox user access.
  * [MailHog](https://github.com/mailhog/MailHog): MailHog is an email testing tool for developers. [smtp](https://github.com/mailhog/smtp)
  * [go-guerilla](https://github.com/flashmob/go-guerrilla)
* [Nodejs](https://nodejs.org/en/) [search](https://github.com/search?l=JavaScript&o=desc&q=smtp&s=stars&type=Repositories)
  * [Haraka](https://github.com/haraka/Haraka): Haraka is a highly scalable node.js email server with a modular plugin architecture. ([configuration](https://github.com/haraka/Haraka#configure-haraka), [plugins](https://github.com/haraka/Haraka/blob/master/config/plugins))
  * [nodemailer smtp-server](https://github.com/nodemailer/smtp-server): Nodemailer is a module for Node.js applications  [docs](https://nodemailer.com/extras/smtp-server/)
* [C, c++](https://en.wikipedia.org/wiki/C_(programming_language)) (searches [C](https://github.com/search?l=C&o=desc&q=smtp&s=stars&type=Repositories), [C#](https://github.com/search?l=C%23&o=desc&q=smtp&s=stars&type=Repositories), [C++](https://github.com/search?l=C%2B%2B&o=desc&q=smtp&s=stars&type=Repositories) )
  * [https://github.com/hmailserver/hmailserver](https://github.com/hmailserver/hmailserver)
* [Python](https://www.python.org/)
  * [https://github.com/modoboa](https://github.com/modoboa)
* others
  * [https://github.com/iredmail/iRedMail](https://github.com/iredmail/iRedMail)

### Appendix B - Implementation Strategy

**Infrastructure Components and Development Tasks**

*Note for the DEES API development we can either enhance the existing Email Alias Service [^6] function to add SMTP administration API's or build a standlone server in go (Option 2). Initial thoughts is option 1 may be the preferred approach however both options have been included below for completeness.*

1. Email Alias Service [^6]
   1. Server: Modify to use DEES API instead of ImprovX
2. ENS-Deployer [^9]
   1. Include Deployment of EAS.sol (optional)
3. go-1ns [^10]
   1. Add EAS.sol abi
   2. Add read methods for Email Alias Services
   3. *Add update methods for Email Alias Service (Option 2 only)*
4. DEES - MSR Plugin (New Development)
   1. Integrate with go-1ns
   2. Add read functionality for Email Alias Service
5. DEES - API SMTP Administration
   1. Email Alias Service - Server [^6] (Option 1)
      1. Develop [API Layer for SMTP administation](#api-layer-smtp-administation)
   2. DEES API - (Option 2)
      1. Build lightweight go server
      2. Integrate with go-1ns
      3. Develop [API Layer for SMTP administation](#api-layer-smtp-administation)

## Footnotes
<!-- Footnotes -->

<!-- Prior Work -->

[^5]: [Project .country](https://github.com/harmony-one/dot-country/blob/main/README.md): the DC contract is meant to provide some "extra service" on top of ENS contracts. Therefore, it charges some additional fees on top of purchasing an ENS domain from the ENS app. (github)

[^6]: [Email Alias Service](https://github.com/harmony-one/dot-country/blob/main/mail.md): The Email Alias Service (EAS) provides .country domain owners email alias addresses which they can privately forward to their existing email addresses. (github)

[^9]: [ens-deployer](https://github.com/harmony-one/ens-deployer): 1NS related contract deployment, customized based on ENS contracts.

[^10]: [go-1ns](https://github.com/jw-1ns/go-1ns): Go module to simplify interacting with the 1 Name Service contracts. Initial version copied from go-ens

<!-- Blockchain email articles -->

<!-- Blockchain email projects -->

<!-- Whitepapers -->

<!-- Github Repositories Blockchain email projects-->

<!-- Additional References and Standards-->

[^61]: [ImprovMX](https://improvmx.com/api/): The [Improvmx](https://improvmx.com/) hosted smtp server api's provide a good reference point for the DEES web3 implementation.

<!-- Maddy email server-->

[^71]: [Maddy Mail Server](https://github.com/foxcpp/maddy): Maddy Mail Server implements all functionality required to run a e-mail server. [Here are the go docs](https://pkg.go.dev/github.com/foxcpp/maddy@v0.6.2#section-readme). Here is an article[Self-Hosted Email With maddy: A Naive First Attempt](https://mediocregopher.com/posts/selfhosted-email-with-maddy). [Here is a comparison with MailCow](https://maddy.email/faq/#how-maddy-compares-to-mailcow-or-mail-in-the-box).

[^72]: [Maddy Modular Design](https://maddy.email/reference/modules/): maddy is built of many small components called "modules". Each module does one certain well-defined task.  This is the [Issue capturing Modular Design](https://github.com/foxcpp/maddy/issues/15).

[^73]: [Maddy SMTP message routing pipeline](https://maddy.email/reference/smtp-pipeline/): Message pipeline is a set of module references and associated rules that describe how to handle messages.

[^74]: [Maddy Envelope sender / recipient rewriting](https://maddy.email/reference/modifiers/envelope/): `replace_sender` and `replace_rcpt` modules replace SMTP envelope addresses based on the mapping defined by the table module (maddy-tables(5)). It is possible to specify 1:N mappings. This allows, for example, implementing mailing lists.

<!-- Additional Items -->