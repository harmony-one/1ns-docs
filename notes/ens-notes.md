# Early Development Note

We only had a rough idea of the .country project at the very beginning, and it was not yet clear to us whether ENS is the best solution. For example, the early iterations of D1DC design do not interact with ENS systems at all. Instead, it uses a simple contract to manage subdomains (of .1.country) mapping to user content (e.g. a tweet). The ENS system was deployed separately while we explore its functionality and evaluate how we can connect web2 features (such as DNS, mail, domain alias, and website setup) and marketplaces (to enable trading of domains) with the web3 system. 

Note that many ENS contracts assume `.eth` is the top-level domain and hard-coded that into various immutable constants. They all need to be changed to accommodate the use cases in Harmony (with `.country` as the top level domain). These changes are already implemented in [`ens-deployer`](https://github.com/harmony-one/ens-deployer) contracts.

## Preliminary notes

The following notes, mostly prepared by John Whitton as he explored ENS, provides references to some resources which may be helpful to people who needed to follow the same path to understand key components of ENS. Learn more at [ens.domains](https://ens.domains/). The build and deployment steps are for exploring ENS itself. For 1NS systems (or to learn more about how ENS is used there), you should always use [`ens-deployer`](https://github.com/harmony-one/ens-deployer) instead of following these steps.

This project should not be confused with [1nsdomains](https://github.com/1nsdomains), a Harmony community-led project.

## Helpful ENS articles

1. [Registering & Renewing Names](https://docs.ens.domains/dapp-developer-guide/registering-and-renewing-names)
2. [Creating Subdomains](https://docs.ens.domains/dapp-developer-guide/managing-names#creating-subdomains)
3. [ENS as NFT](https://docs.ens.domains/dapp-developer-guide/ens-as-nft)). Domains can be traded in Harmony NFT Marketplaces, similar to [ENS on OpenSea](https://opensea.io/collection/ens).
4. [DNS Registrar Guide](https://docs.ens.domains/dns-registrar-guide) (for DNSSEC support)
5. [ENS Enabling your DApp](https://docs.ens.domains/dapp-developer-guide/ens-enabling-your-dapp)

## ENS Technical Components

- Smart Contracts
  - [ens-contracts](https://github.com/ensdomains/ens-contracts): This repo doubles as an npm package with the compiled JSON contracts
    - [ens](https://github.com/ensdomains/ens): Prior version of implementations for registrars and local resolvers for the Ethereum Name Service.
  - [ENS Offchain Resolver](https://github.com/ensdomains/offchain-resolver): smart contracts and a node.js gateway server that together allow hosting ENS names offchain using EIP 3668 and ENSIP 10.
  - [l2gateway-demo](https://github.com/ensdomains/l2gateway-demo/): A demonstration and MVP of an Ethereum <-> Optimism bridge for resolving ENS names (see [medium article](https://medium.com/the-ethereum-name-service/mvp-of-ens-on-l2-with-optimism-demo-video-how-to-try-it-yourself-b44c390cbd67)). Not needed for Harmony use cases, but is worth investegating if we want Harmony to effectively be an L2 for Ethereum ENS.
  - [subdomain-registrar](https://github.com/ensdomains/subdomain-registrar): A set of smart contracts and corresponding web apps that facilitates registration of ENS subdomains.
- SDKs
  - [ensjs-v3](https://github.com/ensdomains/ensjs-v3): The ultimate ENS javascript library, with ethers.js under the hood.
    - [ensjs](https://github.com/ensdomains/ensjs): Prior version of ensjs
- Middleware
  - [ens-avatar](https://github.com/ensdomains/ens-avatar): Avatar resolver library for both nodejs and browser.
    - [ens-avatar-worker](https://github.com/ensdomains/ens-avatar-worker): API [Cloudflare serverless worker](https://developers.cloudflare.com/workers/) for getting avataRs
  - [ens-metadata-service](https://github.com/ensdomains/ens-metadata-service): API to get ens metadata
- Fronted
  - [ens-app-v3](https://github.com/ensdomains/ens-app-v3): The latest version of the ENS app.
    - [ens-app](https://github.com/ensdomains/ens-app): Prior version of the ENS app
- Indexing
  - [ens-subgraph](https://github.com/ensdomains/ens-subgraph): index events from the ENS contracts.
    - See also Harmony's support of [subgraphs](https://docs.harmony.one/home/developers/web3-foundations/tutorials/the-graph-subgraphs)
- Documentation
  - [ENS Documentation](https://github.com/ensdomains/docs): 


### Local Build

Clone and build the following repositories:

- [ens-contracts](https://github.com/ensdomains/ens-contracts): This repo also serves as an npm package with compiled JSON contracts
- [ens-app-v3](https://github.com/ensdomains/ens-app-v3). Follow README for build instructions.
- [ens-metadata-service](https://github.com/ensdomains/ens-metadata-service). See also its own [documentations](https://metadata.ens.domains/docs)
- [ens-avatar-worker](https://github.com/ensdomains/ens-avatar-worker)
- [ens-subgraph](https://github.com/ensdomains/ens-subgraph)
