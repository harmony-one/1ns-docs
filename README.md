# Knowledge Transfer

## Overview

This repository holds information relevant to the design and implementation of Harmony 1ns implementation of ENS.

It is targeted for the Harmony Team and developers.

It is broken into the following sections

| Section | Overview | Audience |
| --- | --- | --- |
| [Design](./design) | Overview of the Harmony 1ns Design | Analysts, Developers |
| [Learning Sessions](./learning) | Educational Material for 1ns Developers | Devlopers |
| [Operations](./operations) | Deployment and Operational information | Operations, Developers |

### **TODO List (Potential)**

- Consolidate Codebase and Production Support
- At some point we should upgrade  @ensdomains/ens-contracts from 15 to 20
  - [v0.0.15](https://www.npmjs.com/package/@ensdomains/ens-contracts/v/0.0.15)  on `^0.0.15`  5 months ago (Oct 2022) cannot find commit in github
  - [Latest](https://www.npmjs.com/package/@ensdomains/ens-contracts?activeTab=explore) is `^0.0.20` [commit](https://github.com/ensdomains/ens-contracts/tree/f15ae2756a795a0a844ad591a406b750749bd1f0) March 1st 2023
  - coredns update
- SMTP <https://github.com/polymorpher/eas>
  - Update of [EAS.sol](https://github.com/polymorpher/eas/blob/main/contract/contracts/EAS.sol)
- [Self Hosted Mail Server](https://github.com/jw-1ns/eas/blob/main/docs/DESIGNV2.md)
- Test all functions in [DC.sol](https://github.com/polymorpher/dot-country/blob/main/contracts/contracts/DC.sol) and  [Tweet.sol](https://github.com/polymorpher/dot-country/blob/main/contracts/contracts/Tweet.sol)
  1. Setup [ens-deployer](https://github.com/polymorpher/ens-deployer) : [service-testing](https://github.com/jw-1ns/ens-deployer/tree/service-testing)
  2. Review Register function in [ensSampleDCDNS.ts](https://github.com/jw-1ns/ens-deployer/blob/service-testing/contract/deploy/ensSampleDCDNS.ts)
  3. Review functions in [DC.sol](https://github.com/polymorpher/dot-country/blob/main/contracts/contracts/DC.sol)
  4. Review functions in [Tweet.sol](https://github.com/polymorpher/dot-country/blob/main/contracts/contracts/Tweet.sol)
  5. Check [config.ts](https://github.com/polymorpher/dot-country/blob/main/contracts/config.ts) and .env have the correct addresses for local ganache instance
  6. Test against local ganache `yarn test —network local`
  7. Develop registration tests for tweet.sol similar to [DNS-001](https://github.com/jw-1ns/ens-deployer/blob/service-testing/contract/test/dns/DNS.ts#LL167C9-L167C16)
  8. Develop url update tests for tweet.sol ([only have sample initialization now](https://github.com/jw-1ns/ens-deployer/blob/service-testing/contract/deploy/ensTweet.ts))
- <https://github.com/jw-1ns/go-1ns> add dns update functionality, improve tests
- Add read functions for tokenId’s to contracts
