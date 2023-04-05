# FAQ

- ERC1155 vs ERC721 and use in ENS

    [EIP721](https://eips.ethereum.org/EIPS/eip-721) Non Fungible Token Standard (sample [ERC721](https://opensea.io/collection/gamaspacestation) a unique character in a game)

    [EIP-1155](https://eips.ethereum.org/EIPS/eip-1155) Multi Token Standard (sample  [ERC1155](https://opensea.io/assets/ethereum/0xa698cf4e59a6e21a97675603e541f1aa5c7d44a3/2) an access pass which can be issued to multiple owners)

    [ENS Name Wrapper](https://docs.ens.domains/dapp-developer-guide/ens-data-guide#namewrapper) [Article](https://web3domains.com/ens-name-wrapper-features-benefits-web3/) [github docs](https://github.com/ensdomains/ens-contracts/tree/master/contracts/wrapper)

    dot-country design notes [JW Daily Log - Harmony ENS](https://www.notion.so/JW-Daily-Log-Harmony-ENS-29f1a291ab1844e9973016d2cb44eb80) (Dec 17th)

  - Creation of Tokens only done in Registrar Controller (code analysis)
    - Creation of Tokens is done in two places [rent in D1DC](https://github.com/jw-ens-domains/dot-country/blob/ens-contracts/contracts/contracts/D1DC.sol#L203) and [register in RegistrarController](https://github.com/jw-ens-domains/ens-deployer/blob/multichain/contract/contracts/RegistrarController.sol#L175) (which overrides register in [BaseRegistrarImplementation](https://github.com/jw-ens-domains/ens-contracts/blob/jw-harmony-build/contracts/ethregistrar/BaseRegistrarImplementation.sol#L121) which used to [mint an ERC721](https://github.com/jw-ens-domains/ens-contracts/blob/jw-harmony-build/contracts/ethregistrar/BaseRegistrarImplementation.sol#L160)) and calls [registerAndWrapETH2LD](https://github.com/jw-ens-domains/ens-contracts/blob/jw-harmony-build/contracts/wrapper/NameWrapper.sol#L257) which [mints](https://github.com/jw-ens-domains/ens-contracts/blob/jw-harmony-build/contracts/wrapper/NameWrapper.sol#L811) an [ERC1155Fuse](https://github.com/jw-ens-domains/ens-contracts/blob/jw-harmony-build/contracts/wrapper/ERC1155Fuse.sol) using [NameWrapper Logic](https://github.com/jw-ens-domains/ens-contracts/tree/jw-harmony-build/contracts/wrapper)
    - D1DC ⇒ ENS NFT
    - ERC721 vs ERC1155 (only needed for subdomains and fuses see [NameWrapper](https://github.com/jw-ens-domains/ens-contracts/blob/jw-harmony-build/contracts/wrapper/NameWrapper.sol)([docs](https://docs.ens.domains/contract-api-reference/name-processing) [README](https://github.com/jw-ens-domains/ens-contracts/tree/jw-harmony-build/contracts/wrapper))) is this something we want to do?
    - D1DC is an ERC721 so this can be removed `//TODO create the EREC721 token at time of construction`
- Research Sources
  - [Ethereum Improvement Proposals](https://eips.ethereum.org/)
