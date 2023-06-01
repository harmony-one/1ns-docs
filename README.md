# Knowledge Transfer

## Overview

Here you can find notes, tutorials, and devops snippets relevant to 1NS, which is initially based on ENS. The resources are targeted for the Harmony Team and ecosystem developers, devided into the following sections

| Section | Overview | Audience |
| --- | --- | --- |
| [Notes](./notes) | Notes taken during active development of .country | Analysts, Developers |
| [Learning Sessions](./learning) | Educational material for 1ns developers | Devlopers |
| [Operations](./operations) | Deployment and operational information | Operations, Developers |

### **TODO List (Tentative)**

- Consolidate Codebase and Production Support
- At some point we should review the latest version of `@ensdomains/ens-contracts`, and update our dependency to a forked repository
	- Our implementation [`ens-deployer`](https://github.com/harmony-one/ens-deployer) is written based on [v0.0.15](https://www.npmjs.com/package/@ensdomains/ens-contracts/v/0.0.15), which appears to exist only as an npm dependency, but not explicitly tagged on Github.
	- As of March 1, 2023, the latest version of [`@ensdomains/ens-contracts`](https://www.npmjs.com/package/@ensdomains/ens-contracts?activeTab=explore) is `^0.0.20` [commit](https://github.com/ensdomains/ens-contracts/tree/f15ae2756a795a0a844ad591a406b750749bd1f0)
	- Update blockchain DNS (CoreDNS 1NS) implementations accordingly, if necessary
- Sending emails via EAS over SMTP <https://github.com/harmony-one/eas>
  - EAS smart contracts may require some improvements, see [EAS.sol](https://github.com/harmony-one/eas/blob/main/contract/contracts/EAS.sol)
- [Self Hosted Mail Server](https://github.com/jw-1ns/eas/blob/main/docs/DESIGNV2.md)
- Test coverage for [DC.sol](https://github.com/harmony-one/dot-country/blob/main/contracts/contracts/DC.sol) and [Tweet.sol](https://github.com/harmony-one/dot-country/blob/main/contracts/contracts/Tweet.sol). 
	- Develop registration tests for tweet.sol similar to [DNS-001](https://github.com/jw-1ns/ens-deployer/blob/service-testing/contract/test/dns/DNS.ts#LL167C9-L167C16)
	- Develop url update tests for tweet.sol ([only have sample initialization now](https://github.com/jw-1ns/ens-deployer/blob/service-testing/contract/deploy/ensTweet.ts))
	- Note: here are the steps to follow for testing:
	  	1. Setup [ens-deployer](https://github.com/harmony-one/ens-deployer) : [service-testing](https://github.com/jw-1ns/ens-deployer/tree/service-testing)
		2. Review register function in [ensSampleDCDNS.ts](https://github.com/jw-1ns/ens-deployer/blob/service-testing/contract/deploy/ensSampleDCDNS.ts)
		3. Review functions in [DC.sol](https://github.com/harmony-one/dot-country/blob/main/contracts/contracts/DC.sol)
		4. Review functions in [Tweet.sol](https://github.com/harmony-one/dot-country/blob/main/contracts/contracts/Tweet.sol)
		5. Check [config.ts](https://github.com/harmony-one/dot-country/blob/main/contracts/config.ts) and .env have the correct addresses for local ganache instance
		6. Test against local ganache `yarn test —network local`
- We may want to add DNS update functionalities in go-1ns module (as part of a CoreDNS 1NS plugin) <https://github.com/harmony-one/go-1ns>, if we want a centralized authority to be able to update DNS records for users' domains (such as to satisfy DNS challenges in generating SSL certificates). Additionally, we need to improve test coverage for this module

