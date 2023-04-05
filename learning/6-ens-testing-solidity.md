# ENS Solidity Development and Testing Working Session (45 mins)

Test all functions in [DC.sol](https://github.com/polymorpher/dot-country/blob/main/contracts/contracts/DC.sol) and  [Tweet.sol](https://github.com/polymorpher/dot-country/blob/main/contracts/contracts/Tweet.sol)

1. Setup <https://github.com/polymorpher/ens-deployer> : [service-testing](https://github.com/jw-1ns/ens-deployer/tree/service-testing)
2. Review Register function in [ensSampleDCDNS.ts](https://github.com/jw-1ns/ens-deployer/blob/service-testing/contract/deploy/ensSampleDCDNS.ts)
3. Review functions in [DC.sol](https://github.com/polymorpher/dot-country/blob/main/contracts/contracts/DC.sol)
4. Review functions in [Tweet.sol](https://github.com/polymorpher/dot-country/blob/main/contracts/contracts/Tweet.sol)
5. Check [config.ts](https://github.com/polymorpher/dot-country/blob/main/contracts/config.ts) and .env have the correct addresses for local ganache instance
6. Test against local ganache `yarn test —network local`
7. Develop registration tests for tweet.sol similar to [DNS-001](https://github.com/jw-1ns/ens-deployer/blob/service-testing/contract/test/dns/DNS.ts#LL167C9-L167C16)
8. Develop url update tests for tweet.sol ([only have sample initialization now](https://github.com/jw-1ns/ens-deployer/blob/service-testing/contract/deploy/ensTweet.ts))
