# ENS Overview (30 minutes)

1. Design Document Review (10 minutes)
2. Repository Structure Review (5 minutes)
3. Repository Review (15 minutes)

TODO

- Document DNS IP management process
- Document frontend integration (web site creation)
- Document SSL certificate provisioning (see [gcp-certs.js](https://github.com/harmony-one/ens-registrar-relay/blob/main/src/gcp-certs.js))

## Releases

| Release | Audience | Description | Design Docs |
| --- | --- | --- | --- |
| wallet | External | non-custodial wallet infrastructure (built seperately prior to ens and can be leveraged by ENS moving forward) | [one-wallet](https://github.com/polymorpher/one-wallet/blob/master/README.md) modulo.so One Time Password (OTP) Wallet </br> [SMS Controlled Mini-wallet](https://github.com/polymorpher/sms-wallet/wiki#sms-controlled-mini-wallet) |
| v0 | Internal | ens evaluation for Harmony |  [1NS Deployment](./1NS-DEPLOYMENT.md): Copy of [ENS Documentation: Deploying ENS on a private chain](https://docs.ens.domains/deploying-ens-on-a-private-chain) ([github](https://github.com/ensdomains/docs/blob/master/deploying-ens-on-a-private-chain.md)), starting point for deploying dot-country ens contracts on Harmony. </br> [Harmony Name Service Design Document](.//1NS-DESIGN.md): This document covers the high level development tasks to create similar functionality as ens.domains for Harmony. |
| v1.0 | External | .1.country Close-ended product for .1.country domain only | [.1.country](https://github.com/polymorpher/.1.country#readme) |
| v2.0 | External | dot county web2 and web3 domain registration with web2 frontend | [dot-country](https://github.com/harmony-one/dot-country/blob/main/README.md) |
| v3.0 | External | dot-country with web3 backend for DNS management | [Domain Name Service Architecture](../design/DNS-ARCHITECTURE.md):review of existing DNS services and proposed architecture for Harmony  </br> [core-dns web3 plugin design](../design/CORE-DNS.md): development framework and build process to develop a web3 backend [plugin](https://coredns.io/explugins/) for [core-dns](https://coredns.io/).|
| v4.0 | External | dot-country with email alias services supporting SMTP management | [Decentralized Encrypted Email Service (DEES)](./DEES.md): draft product suite design </br> [Email Alias Service Design](./EAS-DESIGN.md): requirements and design for an Email Alias Service. </br> [Email Alias Service Implementation](./EAS-IMPLEMENTATION.md): implementation strategy for implementing smtp services with a web3 backend. </br> [Email Alias Service Developer.md](./EAS-DEVELOPER.md): EAS contributors guide |

## Design documents

Following are the design documents for dot-country

*Note: These are currently under review with the goal of a consolidated design document.*

Following are the design documents for dot-country

*Note: These are currently under review with the goal of a consolidated design document.*

- [1NS Deployment](../design/1NS-DEPLOYMENT.md): Copy of [ENS Documentation: Deploying NS on a private chain](https://docs.ens.domains/deploying-ens-on-a-private-chain) ([github](https://github.com/ensdomains/docs/blob/master/deploying-ens-on-a-private-chain.md)), used a starting point for deploying dot-country ens contracts on Harmony.
- [Harmony Name Service Design Document](../design/1NS-DESIGN.md): This document covers the high level development tasks to create similar functionality as ens.domains for Harmony.
- [Domain Name Service Architecture](../design/DNS-ARCHITECTURE.md): This document gives a review of existing DNS services and proposes an architecture for Harmony Domain Name Service.
- [core-dns web3 plugin design](../design/CORE-DNS.md): This document gives an overview of the development framework and build process to develop a web3 backend [plugin](https://coredns.io/explugins/) for [core-dns](https://coredns.io/).
- [Decentralized Encrypted Email Service (DEES)](../design/DEES.md): This document provides a design and development framework for developing a decentralized encrypted email service integrated with web3 domain name services.
- [Email Alias Service Design](../design/EAS-DESIGN.md): This document provides high level requirements and design for an Email Alias Service.
- [Email Alias Service Implementation](../design/EAS-IMPLEMENTATION.md): This document provides an analysis of open source SMTP services and an implementation strategy for implementing similar services with a web3 backend.
- [Email Alias Service Developer.md](../design/EAS-DEVELOPER.md): This guide gives an overview for contributors on how to fork and develop on EAS. For an understanding of the design of EAS please review [DESIGN.md](../design/EAS-DESIGN.md).

## Technical Overview

Following is an overview of the repositories developed or updated as part of the dot-country initiative.

Repositories have been developed under 3 organizations

- [jw-1ns](https://github.com/jw-1ns): contains
  - original repositories for design of dot-country and dns server functionality (coredns plugin)
  - forks of [ensdomains](https://github.com/ensdomains) repositories used in inital evaluation and development.
  - forks of [polymorpher](https://github.com/polymorpher/) customized dot-country development
- [polymorpher](https://github.com/polymorpher/): contains
  - original repositories for dot-country devlopment
  - forks of [jw-1ns](https://github.com/jw-1ns) dns server functionality
- [harmony-one](https://github.com/harmony-one): contains
  - original repositories for dot-country frontend and utilities as well as code modifications to harmony explorer for dot-country
  - forks of [polymorpher](https://github.com/polymorpher/) dot-country repositories
  - forks of [jw-1ns](https://github.com/jw-1ns) coredns repositories

The goal is to migrate all these repositories under the harmony-one organization and moving forward any development would be initiated by forking this repository and creating pull request to the harmony-one repository for new work.

*Note: this development relies on a number of repositories from [Ethereum Name Service (ENS)](https://github.com/ensdomains) for web3 backend functionality and other utilities*

### Registration Flow

![Registration Flow](/assets/registration-flow.png "Registration Flow")

### Architecture

![DEES Reference Architecture](/assets/dees-architecture.jpeg "DEES Reference Architecture")

## Codebase by Release

### Wallet - non custodial wallet infrastructure

| Component | Sub-Component | Repository | Description |
| --- | --- | --- | --- |
| Wallet | one-wallet | [one-wallet](https://github.com/polymorpher/one-wallet) |1wallet is an unconventional keyless, non-custodial smart contract wallet. |
| Wallet | sms-wallet | [sms-wallet](https://github.com/polymorpher/sms-wallet) | The SMS Wallet is a lightweight non-custodial wallet solution for users to quickly create a wallet bound to their phone number, receive (or purchase) inexpensive NFTs, showcase their NFTs, and to hold a small amount of crypto assets. |

### v0 - ens evaluation for Harmony

| Component | Sub-Component | Repository | Description |
| --- | --- | --- | --- |
| Web3 | ens | [ens-deployer](https://github.com/harmony-one/ens-deployer) | 1NS related contract deployment, customized based on ENS contracts. |
| Web3 | Metadata | [ens-metadata-service](https://github.com/harmony-one/ens-metadata-service) | Retrieves Metadata information from IPFS for a given TokenId |
| Web3 | Reporting | [graph-node](https://github.com/jw-1ns/graph-node) |  protocol for building decentralized applications (dApps) quickly on Ethereum and IPFS using GraphQL.|
| Web2 | Avatar Storage | [ens-avatar-worker](https://github.com/harmony-one/ens-avatar-worker) | [Cloudflare Worker](https://developers.cloudflare.com/workers/) used to store avatars |
| Frontend | Domain Registration | [ens-app-v3](https://github.com/harmony-one/ens-app-v3) | The all new, all cool version of the ENS manager.|
| Reporting | ens | [ens-subgraph](https://github.com/harmony-one/ens-subgraph) | This Subgraph sources events from the ENS contracts. This includes the ENS registry, the Auction Registrar, and any resolvers that are created and linked to domains.|

### v1.0 - .1.country Close-ended product for .1.country domain only  

| Component | Sub-Component | Repository | Description |
| --- | --- | --- | --- |
| Mono-repo | .1.country | [.1.country](https://github.com/harmony-one/.1.country) | Close-ended product for .1.country |

### v2.0 - dot county web2 and web3 domain registration with web2 frontend

| Component | Sub-Component | Repository | Description |
| --- | --- | --- | --- |
| Web3 | ens | [ens-deployer](https://github.com/harmony-one/ens-deployer) | 1NS related contract deployment, customized based on ENS contracts. |
| Web3 | 1-country | [1-country.contract](https://github.com/harmony-one/1-country.contract) | A domain manager contract for .country (DC -  Dot Country) |
| Web3 | relay | [ens-registrar-relay](https://github.com/harmony-one/ens-registrar-relay) | ENS Registrar Relay is a backend server that communicates with web2 registrars on behalf of web3 clients (e.g. browsers). Is also responsible for [SSL certificate provisioning](https://github.com/harmony-one/ens-registrar-relay/blob/main/src/gcp-certs.js). |
| Web3 | Explorer Backend | [explorer-v2-backend](https://github.com/harmony-one/explorer-v2-backend) | Harmony Blockchain Data Indexer |
| Web2 | DNS Storage|[coredns-redis](https://github.com/harmony-one/coredns-redis) | DNS Server plugin using [redis](https://redis.io/) open source, in-memory data store |
| Web2 | NFT Image Storage | [nft-images-generator](https://github.com/harmony-one/nft-images-generator) | an API that generates an NFT image by overlaying text on a background image and uploads it to a designated Google Storage bucket location |
| Frontend | Block Explorer | [explorer-v2-frontend](https://github.com/harmony-one/explorer-v2-frontend) | Harmony Block Explorer frontend |
| Frontend | 1.country | [1-country.frontend](https://github.com/harmony-one/1-country.frontend) | 1.country allows users to claim a Web3 name .1 that also directs to a browsable Web2 domain .country. |
| Mono-repo | dot-country | [dot-country](https://github.com/harmony-one/dot-country) | dot-country mono repository containing frontend client and dot-country contracts which interact with ens contracts |

### v3.0 - dot-country with web3 backend for DNS management

Includes all components in v2.0 except for coredns-redis which is to be replaced by coredns-1ns migrating dns storage to web3.

Plus these additional components

| Component | Sub-Component | Repository | Description |
| --- | --- | --- | --- |
| Utility | Golang SDK for ens web3 | [go-1ns](https://github.com/harmony-one/go-1ns) | Go module to simplify interacting with the Harmony Name Service contracts. Initial version copied from go-ens. |
| Utility | DNS-JS | [node-dns-js](https://github.com/polymorpher/node-dns-js) | NPM module allowing DNS packet parsing |
| Utility | DNS-JS | [1ns-node-dns-js](https://github.com/jw-1ns/1ns-node-dns-js) | NPM module allowing DNS packet parsing |
| Web3 | DNS server Plugin using web3 backend | [coredns-1ns](https://github.com/harmony-one/coredns-1ns) | DNS server plugin using web3 backend of ens-contracts. Interacts with web3 backend via [go-1ns](https://github.com/harmony-one/go-1ns) |

### v4.0 - dot-country with email alias services supporting SMTP management

| Component | Sub-Component | Repository | Description |
| --- | --- | --- | --- |
| Mono-repo | Email Alias Service | [eas](https://github.com/harmony-one/eas) | The Email Alias Service (EAS) provides .country domain owners email alias addresses which they can privately forward to their existing email addresses. |

### For Review and/or archiving

| Component | Sub-Component | Repository | Description |
| --- | --- | --- | --- |
| Web3 | Subdomain registrar | [subdomain-registrar-core](https://github.com/polymorpher/subdomain-registrar-core) | contains only the contract code from @ensdomains/subdomain-registrar, so other projects can easily import the contract by adding this project as a dependency |
| Web2 | Video Storage | [mux-uploader.backend](https://github.com/harmony-one/mux-uploader.backend) | Decentralized storage layer for videos using aws, [storj](https://www.storj.io/), web3 authorizations (to JWT) and a dagtabase. |
| Utility | one country sdk | [one-country-sdk](https://github.com/harmony-one/one-country-sdk) | Web3 library for 1.country smart contracts which can be installed as an npm package for front end clients. |
| Frontend | ens | [ens-app](https://github.com/harmony-one/ens-app) | ENS Frontend Application |
| Frontend | Adresses | [react-ens-address](https://github.com/harmony-one/react-ens-address) | Drop-in React component to resolve ENS names or provide feedback with reverse records.|
| Frontend | dot-country domain component |  [dot-country-manager](https://github.com/harmony-one/dot-country-manager) | Front End component showing dot-country domain metadata |
| Frontend | One redirect browser extension | [one-redirect](https://github.com/harmony-one/one-redirect) | Support redirect xx.1 to xx.1.country even tho the browser does not recognized .1 domain. |

### Demonstration Videos

- [local development ens-app-v3](https://www.notion.so/eavenetwork/Harmony-dot-country-knowledge-transfer-159bf0bd62844b519c71f36122e7c07f): provides an overview of all the components used in initial local ens-app development.
- [local docker development ens-app-v3](https://www.notion.so/eavenetwork/Harmony-dot-country-knowledge-transfer-159bf0bd62844b519c71f36122e7c07f): provides an overview for using docker for local ens development.

### Deployment Overview

![Deployment Overview](/assets/dot-country-deployment.png "Registration Flow")

### Storage Overview

![Storage Overview](/assets/dot-country-storage.png "Storage Overview")
