# Solidity Development (45 minutes)

1. Solidity Tooling Review (10 minutes)
2. Local Development (35 minutes)

## Solidity Tools

- [Remix](https://remix.ethereum.org/)
- [Truffle](https://trufflesuite.com/docs/)
- [Ganache](https://trufflesuite.com/ganache/) (app)
- [Ganache-cli](https://www.npmjs.com/package/ganache) (npm)
- [Hardhat](https://hardhat.org/docs) ([npm package](https://www.npmjs.com/package/hardhat))
- [Hardhat-deploy](https://github.com/wighawag/hardhat-deploy#readme) ([npm package](https://www.npmjs.com/package/hardhat-deploy))
- [OpenZeppelin](https://docs.openzeppelin.com/) ([contracts](https://docs.openzeppelin.com/contracts/4.x/), [upgradeable](https://docs.openzeppelin.com/contracts/4.x/upgradeable), [upgrade plugin](https://docs.openzeppelin.com/upgrades-plugins/1.x/))
- [Foundry](https://book.getfoundry.sh/) ([rust](https://book.getfoundry.sh/faq?highlight=rust#how-to-install-from-source))

## Local Development Environment

### Globally Installed npm packages

```bash
# Help Information
npm -h                  //npm usage
npm -l                  //display usage info for all commands
npm ls -h               //display usage for specific command

# Listing Packages

npm list -g -depth 0
/Users/johnwhitton/.nvm/versions/node/v19.7.0/lib
├── corepack@0.16.0
├── eslint@8.35.0
├── ganache@7.7.5
├── npm@9.5.0
├── pnpm@8.1.0
├── ts-loader@9.4.2
└── yarn@1.22.199
```

### [Node Versions](https://nodejs.dev/en/about/releases/) ([previous](https://nodejs.org/en/download/releases)) and [Node Version Manager(nvm)](https://github.com/nvm-sh/nvm#readme)

- sample commands

  ```bash
  # help information
  nvm -h
  
  # install
  nvm install 8.0.0
  
  # Default Version
  nvm alias default 8.1.0
  
  # Setting version
  nvm use v18.14.2
  
  # Sample alias to cd and set node version (add to .bash_profile)
  alias cd1c="cd ~/1ns/ens-deployer/contract;nvm use v18.14.2; pwd"
  ```

- define required version in [package.json](https://github.com/harmony-one/ens-deployer/blob/main/contract/package.json#L5-L7) using [engines](https://docs.npmjs.com/cli/v9/configuring-npm/package-json#engines)

  ```bash
  {
      "name": "ens-deploy",
      "version": "0.0.1",
      "license": "Apache-2.0",
      "engines": {
          "node": ">=16"
      },
  
  ...
  }
  ```

### Ganache

- [Ganache](https://www.npmjs.com/package/ganache) CLI usage ([new](https://github.com/harmony-one/ens-deployer/blob/main/env/ganache-new.sh), [restart](https://github.com/harmony-one/ens-deployer/blob/main/env/ganache-restart.sh))
  - sample commands

    ```bash
    # help information
    ganache -h
    
    # starting ganache with default values and random mnemonic
    ganache
    
    # Sample command Instamining
    ganache --server.ws --database.dbPath "./db" -m "test test test test test test test test test test test junk" -e 100000
    
    # Sample command block time of 2 seconds
    ganache -b 2 --server.ws --database.dbPath "./db" -m "test test test test test test test test test test test junk" -e 100000
    ```

  - mnemonic: can be used for consistent frontend testing and metamask interaction
            1. Run ganache
            2. Get the private key of the account
            3. Add the account to metamask (import account) **Never use for prod**
            4. Add the Network to metamask (settings ⇒ networks ⇒ add network)
            5. Reset account whenever restarting ganache (fixes nonce issues)
  - instamining: default is instamining, but can also set blocktime `-b 2`
  - datadir: usually default to the current directory (add to .gitignore)
  - account balances: can set initial account balances (useful when doing expensive tests (e.g. registering 1 character domains) `-e 100000`

### [Yarn](https://yarnpkg.com/) ([npm package](https://www.npmjs.com/package/yarn))

### Hardhat Tools

- [Plugins](https://hardhat.org/hardhat-runner/plugins)
- [Sample Project](https://github.com/johnwhitton/bc_template): [docs](https://github.com/johnwhitton/bc_template/blob/main/docs/README.md) [hardhat_config](https://github.com/johnwhitton/bc_template/blob/main/hardhat.config.ts)
- [Example ens build using slither](https://github.com/jw-1ns/ens-contracts) [hardhat_config](https://github.com/jw-1ns/ens-contracts/blob/jw-harmony-build/hardhat.config.ts) [package.json](https://github.com/harmony-one/ens-deployer/blob/main/contract/package.json)

  ```bash
  # clone the project
  git clone https://github.com/jw-1ns/ens-contracts
  cd ens-contracts/
  git checkout jw-harmony-build
  
  # Install the node version if needed
  nvm install 16.19.1
  
  # Set the node version
  nvm use 16.19.1
  
  # install slither if needed
  pip3 install slither-analyzer
  export PATH="$PATH:~/Library/Python/3.9/bin"
  
  # install xdot if needed https://formulae.brew.sh/formula/xdot
  brew install xdot
  
  # install npm packages
  yarn install
  
  # Compile the contracts
  yarn compile
  
  # yarn test
  # Show gas usage
  REPORT_GAS=true npx hardhat test
  
  # check slither warnings
  yarn slither
  
  # create graphs via slither
  yarn slither-inheritance
  yarn slither-resolver
  
  # deploy on hardhat
  yarn deploy
  
  # deploy on ganache
  yarn deploy --network local
  ```

### [Contract Verification](https://hardhat.org/hardhat-runner/docs/guides/verifying): [example](https://explorer.harmony.one/address/0x034a4ace40dacaf40e5392bf55867d0228307beb?activeTab=6)

- `npx hardhat flatten contracts/ENSDeployer.sol`
- [Sample](https://github.com/harmony-one/contract-verification-service/issues/31)
- <https://github.com/KangaFinance/hardhat> [harmony support](https://github.com/KangaFinance/hardhat/commits/master)
- [Sample Harmony Verification](https://github.com/harmony-one/contract-verification-service/issues/31)
- Proxy Deployment
- [ERC-1967: Proxy Storage Slots](https://eips.ethereum.org/EIPS/eip-1967)
- [ERC-2535: Diamonds, Multi-Facet Proxy](https://eips.ethereum.org/EIPS/eip-2535)
- [openzeppelin: Proxy Upgrade Pattern](https://docs.openzeppelin.com/upgrades-plugins/1.x/proxies)
- [openzepplin: OpenZeppelin Hardhat Upgrades API](https://docs.openzeppelin.com/upgrades-plugins/1.x/api-hardhat-upgrades)
- [hardhat deploy: Deploying and Upgrading Proxies](https://github.com/wighawag/hardhat-deploy#deploying-and-upgrading-proxies)  <https://github.com/wighawag/hardhat-deploy/issues/146> [comment](https://github.com/wighawag/hardhat-deploy/issues/146#issuecomment-1238731556)
- [hardhat deploy: Builtin-In Support For Diamonds (EIP2535)](https://github.com/wighawag/hardhat-deploy#builtin-in-support-for-diamonds-eip2535)
- Example: <https://github.com/polymorpher/sms-wallet>  [proxy-upgrade](https://github.com/polymorpher/sms-wallet/tree/proxyUpgrade/miniwallet/deploy)  <https://github.com/wighawag/hardhat-deploy/issues/146> [comment](https://github.com/wighawag/hardhat-deploy/issues/146#issuecomment-1238731556)
- Testing [Waffle](https://ethereum-waffle.readthedocs.io/en/latest/) (npm package)
- [truffle](https://trufflesuite.com/docs/truffle/) tests ([solidity](https://trufflesuite.com/docs/truffle/how-to/debug-test/write-tests-in-solidity/) [javascript](https://trufflesuite.com/docs/truffle/how-to/debug-test/write-tests-in-javascript/))
- [truffle develop](https://trufflesuite.com/docs/truffle/how-to/debug-test/use-truffle-develop-and-the-console/) : interactive console using ganache
- [hardhat (javascript tests)](https://hardhat.org/tutorial/testing-contracts) [mocha](https://mochajs.org/) [waffle](https://ethereum-waffle.readthedocs.io/en/latest/matchers.html)
- [hardhat debugging](https://hardhat.org/tutorial/debugging-with-hardhat-network) ([npm](https://www.npmjs.com/package/hardhat-console)): [console.log commands](https://hardhat.org/hardhat-network/docs/reference#console.log) : logging for hardhat (not local)
- Samples
  - ‣ : [Smart Contract Testing Guide](https://github.com/polymorpher/one-wallet/blob/master/code/test/README.md) (truffle, [chai](https://www.chaijs.com/api/assert/))
  - ‣: [Smart Contract Testing Guide](https://github.com/polymorpher/sms-wallet/blob/main/miniwallet/test/README.md) (hardhat, waffle, ethers)
  - ‣: [ens-deployer tests](https://github.com/polymorpher/ens-deployer/blob/main/contract/test/README.md) (hardhat, waffle, ethers)

### Javascript [web3](https://web3js.readthedocs.io/en/v1.8.2/) vs [ethers](https://docs.ethers.org/v5/) and [ethereumjs-util](https://www.npmjs.com/package/ethereumjs-util)

- [Blockchain councilWeb3.js Vs Ethers.js : Know the Key Differences](https://www.blockchain-council.org/web-3/web3-js-vs-ethers-js/)
- [The Distinctions between Ethers.js and Web3.js](https://medium.com/coinmonks/the-distinctions-between-ethers-js-and-web3-js-8e51f60083ce)
- [ethereumjs-util](https://www.npmjs.com/package/ethereumjs-util): A collection of utility functions for Ethereum. (hex, rlp, etc)
- Providers
  - ethers ([Provider](https://docs.ethers.org/v5/api/providers/), [signer](https://docs.ethers.org/v5/api/signer/), c[onnecting to contracts](https://docs.ethers.org/v5/api/contract/example/#example-erc-20-contract--connecting-to-a-contract)):

      If you are coming from Web3.js, this is one of the biggest differences you will encounter using ethers.

      The ethers library creates a strong division between the operation a Provider can perform and those of a Signer, which Web3.js lumps together.

      This separation of concerns and a stricted subset of Provider operations allows for a larger variety of backends, a more consistent API and ensures other libraries to operate without being able to rely on any underlying assumption.

      If a Provider is given, the contract has only read-only access, while a Signer offers access to state manipulating methods.

  - [web3](https://web3js.readthedocs.io/en/v1.8.2/web3.html#providers)

### Contract interaction

- [web3](https://web3js.readthedocs.io/en/v1.8.2/web3-eth-contract.html) : The web3.eth.Contract object makes it easy to interact with smart contracts on the ethereum blockchain. When you create a new contract object you give it the json interface of the respective smart contract and web3 will auto convert all calls into low level ABI calls over RPC for you.
- [ethers](https://docs.ethers.org/v5/api/contract/): A Contract object is an abstraction of a contract (EVM bytecode) deployed on the Ethereum network. It allows for a simple way to serialize calls and transactions to an on-chain contract and deserialize their results and emitted logs.

    A ContractFactory is an abstraction of a contract's bytecode and facilitates deploying a contract.

### Utilities

- [web3](https://web3js.readthedocs.io/en/v1.8.2/web3-utils.html)
- [ethers](https://docs.ethers.org/v5/api/utils/)
  
### Sample Libraries

- [ethereumjs-util](https://www.npmjs.com/package/ethereumjs-util): A collection of utility functions for Ethereum. (hex, rlp, etc)
- <https://github.com/ensdomains/ensjs-v3>: [utils](https://github.com/ensdomains/ensjs-v3/tree/main/packages/ensjs/src/utils) (typescript)
- <https://github.com/ensdomains/ens-contracts>: [utils](https://github.com/ensdomains/ens-contracts/tree/master/contracts/utils) (solidity)
- <https://github.com/polymorpher/one-wallet>: [lib](https://github.com/polymorpher/one-wallet/tree/master/code/lib) [constants](https://github.com/polymorpher/one-wallet/blob/master/code/lib/constants.js) [onewallet.js](https://github.com/polymorpher/one-wallet/blob/master/code/lib/onewallet.js) (javascript)
- [lodash](https://lodash.com/docs/4.17.15) : ([npm](https://www.npmjs.com/package/lodash))
- [chai assertions](https://www.chaijs.com/api/assert/)
