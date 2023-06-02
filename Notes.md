# Notes 

- Discussion and development notes (John Whitton): [JW Daily Log - Harmony ENS](https://www.notion.so/JW-Daily-Log-Harmony-ENS-29f1a291ab1844e9973016d2cb44eb80)
- ERC1155 vs ERC721 are both used in ENS, representing two forms of NFTs of the same domain. When a domain is registered, both NFTs are minted. The user gains ownership over one of the NFTs at a time, and can convert between them at any time.
    - [EIP-721](https://eips.ethereum.org/EIPS/eip-721) is Non Fungible Token Standard. See sample [ERC721](https://opensea.io/collection/gamaspacestation) - representing a unique character in a game
    - [EIP-1155](https://eips.ethereum.org/EIPS/eip-1155) is Multi Token Standard. See sample [ERC1155](https://opensea.io/assets/ethereum/0xa698cf4e59a6e21a97675603e541f1aa5c7d44a3/2) - representing an access pass which can be issued to multiple owners.
- [NameWrapper](https://docs.ens.domains/dapp-developer-guide/ens-data-guide#namewrapper) is the contract that implements the ERC1155 standard, and manages the conversion through "wrap" and "unwrap" related functions. To learn more, see the [article](https://web3domains.com/ens-name-wrapper-features-benefits-web3/) and [developer docs](https://github.com/ensdomains/ens-contracts/tree/master/contracts/wrapper).
- The tokens representing the domains were minted in TLDNameWrapper and TLDBaseRegistrarImplementation contracts. The process is initiated by the RegistrarController contract.
- DC contract relies on RegistrarController to register the domain (and query for pricing), but interacts directly with TLDNameWrapper and TLDBaseRegistrarImplementation to determine ownership.
- To explore the call sequence, it is best to install Solidity plugins in your IDE (e.g. Webstorm or VS Code), and make uses of the "Go to Definition" functionality
