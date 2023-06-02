# CoreDNS Web3 Plugin

- [core-dns web3 plugin design](#core-dns-web3-plugin-design)
  - [Overview](#overview)
  - [Development Components](#development-components)
    - [1ns-contracts](#1ns-contracts)
    - [1ns-go](#1ns-go)
    - [coredns-1ns](#coredns-1ns)
    - [core-dns](#core-dns)
  - [References](#references)
    - [core-dns (go) - Evaluation](#core-dns-go---evaluation)

## Overview

Here are the notes taken during the development of a web3 [plugin](https://coredns.io/explugins/) for [CoreDNS](https://coredns.io/). When the plugin is activated, the CoreDNS server will query the blockchain to lookup DNS records and to answer incoming DNS queries.

## Components

### [go-1ns](https://github.com/harmony-one/go-1ns)

The package provides a library to interact with 1NS smart contracts (see ens-deployer). The contract Go implementations are automatically generated based on the ABIs. See <https://github.com/harmony-one/go-1ns/tree/main/contracts/baseregistrar> for example. The intial version was based on [go-ens](https://github.com/wealdtech/go-ens).

### [coredns-1ns](https://github.com/harmony-one/coredns-1ns)

**Summary**

This package implements the aforementioned [CoreDNS plugin](https://coredns.io/2016/12/19/writing-plugins-for-coredns/). Similar to the ENS CoreDNS plugin described in [this article](http://www.wealdtech.com/articles/ethdns-an-ethereum-backend-for-the-domain-name-system/), this plugin works with 1NS contracts and systems, and is compatible with Harmony network.

An auto-generated barebone documentation for the package can be found [here](/assets/go-1ns-docs.pdf)

**Sample Query Workflow**

Here is an end-to-end flow of handling an A record DNS query, and how the plugin interacts with the blockchain.

1. core-dns-1ns performs a [Query](https://github.com/harmony-one/coredns-1ns/blob/main/1ns.go#L66)

   `func (e 1NS) Query(domain string, name string, qtype uint16, do bool) ([]dns.RR, error) {`

2. for entry types SOA,NS,TXT,A,AAAA it performs [obtainContentHash](https://github.com/harmony-one/coredns-1ns/blob/main/1ns.go#L80)

    `contentHash, err = e.obtainContentHash(name, domain)`

3. for A record types if calls [e.handleA](results, err = e.handleA(name, domain, contentHash)

   `results, err = e.handleA(name, domain, contentHash)`

4. handleA calls [e.obtainARRSet(name, domain)](https://github.com/harmony-one/coredns-1ns/blob/main/1ns.go#L224)

   `aRRSet, err := e.obtainARRSet(name, domain)`

5. obtainARRSet calls [getDNSResolver](https://github.com/harmony-one/coredns-1ns/blob/main/1ns.go#L310) to get the resolver

   `resolver, err := e.getDNSResolver(ethDomain)`

6. getDNSResolver calls [NewDNSResolver](https://github.com/harmony-one/coredns-1ns/blob/main/1ns.go#L368)

   `resolver, err := 1ns.NewDNSResolver(e.Client, domain)`

   1. 1ns.NewDNSResolver calls [NewDNSResolver](https://github.com/harmony-one/go-1ns/blob/main/dnsresolver.go#L38) in 1ns-go

      `func NewDNSResolver(backend bind.ContractBackend, domain string) (*DNSResolver, error)`

      1. NewDNSResolver creates a [NewRegistry](https://github.com/harmony-one/go-1ns/blob/main/dnsresolver.go#L39) instance

            `registry, err := NewRegistry(backend)`

      2. The registry is used to get the [ResolverAddress](https://github.com/harmony-one/go-1ns/blob/main/dnsresolver.go#L43) using 1ns-go to get the resolvers contract address

            `address, err := registry.ResolverAddress(domain)`

      3. [ResolverAddress](https://github.com/harmony-one/go-1ns/blob/main/registry.go#L69) is a function in registry.go which interacts with the Registry contract to get the Resolver address

            `return r.Contract.Resolver(nil, nameHash)`

            1. [resolver](https://github.com/ensdomains/ens-contracts/blob/master/contracts/registry/ENSRegistry.sol#L170) is a function in ENSRegistry.sol in ens-contracts whch returns the resolvers address based on the hash passed

                ```
                function resolver(bytes32 node) public view virtual override returns (address) {
                    return records[node].resolver;
                }
                ```

      4. ResolverAddress is used to create a [NewDNSResolverAt](https://github.com/harmony-one/go-1ns/blob/main/dnsresolver.go#L48) Resolver contract instance

            `return NewDNSResolverAt(backend, domain, address)`

7. obtainARRSet calls [resolver.Record](https://github.com/harmony-one/coredns-1ns/blob/main/1ns.go#L315) to get the A record

      `return resolver.Record(name, dns.TypeA)`

      1. resolver.Record calls [Record](https://github.com/harmony-one/go-1ns/blob/main/dnsresolver.go#L76) in 1ns.go

            `func (r *DNSResolver) Record(name string, rrType uint16) ([]byte, error) {`

         1. Record calls [r.Contract.DnsRecord](https://github.com/harmony-one/go-1ns/blob/main/dnsresolver.go#L81)

            `return r.Contract.DnsRecord(nil, nameHash, DNSWireFormatDomainHash(name), rrType)`

            1. [dnsRecord](https://github.com/ensdomains/ens-contracts/blob/master/contracts/resolvers/profiles/DNSResolver.sol#L117) is a function in DNSResolver.sol in ens-contracts

               ```
               function dnsRecord(bytes32 node, bytes32 name, uint16 resource) public view virtual override returns (bytes memory) {
                    return versionable_records[recordVersions[node]][node][name][resource];
                }
                ```

**Helpful Links**

- [How to Add Plugins to CoreDNS](https://coredns.io/2017/03/01/how-to-add-plugins-to-coredns/)
- [Compile Time Enabling or Disabling Plugins](https://coredns.io/2017/07/25/compile-time-enabling-or-disabling-plugins/)

**TODOs**

1. Update [1ns.go](https://github.com/harmony-one/coredns-1ns/blob/main/1ns.go) to reflect updated [go-1ns](#go-1ns) package.
2. Revise [server.go](https://github.com/harmony-one/coredns-1ns/blob/main/server.go)
3. End-to-end testing locally using [build-standalone.sh](https://github.com/harmony-one/coredns-1ns/blob/main/build-standalone.sh)
4. Document the plugin (see [ens](https://coredns.io/explugins/ens/))
5. Test and iterate in production on Harmony Mainnet

**References**

<a name="cd1">[1]</a>  [CoreDNS Plugin Approach](https://coredns.io/2017/03/01/how-to-add-plugins-to-coredns/): CoreDNS is a DNS server that chains plugins. A plugin is defined as a method: ServeDNS() that gets a request and either responds to the client or passes it on to the next plugin. If none of the plugins handle the request a default response of SERVFAIL is returned.

<a name="cd2">[2]</a>  [CoreDNS Enabling Plugins](https://coredns.io/2017/07/25/compile-time-enabling-or-disabling-plugins/): CoreDNS' plugins (or external plugins) can be enabled or disabled on the fly by specifying (or not specifying) it in the Corefile. But you can also compile CoreDNS with only the plugins you need and leave the rest completely out.

<a name="cd3">[3]</a>  [CoreDNS Developed Plugins](https://coredns.io/plugins/): Provides a list of all Plugins developed by CoreDNS. source code can be found on [github](https://github.com/coredns/coredns/tree/master/plugin).

<a name="cd4">[4]</a>  [CoreDNS External Plugins](https://coredns.io/explugins/): Provides a list of External Plugins.

<a name="cd5">[5]</a>  [ENS Plugin](https://coredns.io/explugins/ens/): The ens plugin serves DNS records from the Ethereum Name Service. Source code can be found on [github](https://github.com/wealdtech/coredns-ens). With a detailed description found in [EthDNS: an Ethereum backend for the Domain Name System](http://www.wealdtech.com/articles/ethdns-an-ethereum-backend-for-the-domain-name-system/)

> The nameservers for the my.xyz domain have been configured to fetch information not from a local store but from the Ethereum blockchain, shown as the Ethereum symbol in the bottom-right corner of the diagram. Note that the name resolution process is exactly the same as in the previous diagram, and that the client is unaware that the information has been returned from the blockchain. This makes EthDNS a drop-in replacement for existing nameservers, working seamlessly with the existing infrastructure.

<a name="cd6">[6]</a> [go-ens](https://github.com/wealdtech/go-ens): Go module to simplify interacting with the Ethereum Name Service contracts.

<a name="cd7">[7]</a> [ens-contracts](https://github.com/ensdomains/ens-contracts): This repo doubles as an npm package with the compiled JSON contracts for ENS.

<a name="cd8">[8]</a> [acl-plugin](https://coredns.io/plugins/acl/): acl enforces access control policies on source ip and prevents unauthorized access to DNS servers. Source code is located on [github](https://github.com/coredns/coredns/tree/master/plugin/acl).

<a name="cd9">[8]</a>  [A first look at CoreDNS](https://jpmens.net/2017/09/09/coredns/): Initial review of coreDNS functionality.

<a name="cd10">[9]</a>  [EPP Integration Approach](https://github.com/coredns/coredns/issues/131):
> As much as I would love to have an EPP client written in go, it isn't likely that an EPP client would be useful to many users. Only registrars typically have access to the EPP endpoints at registries. And even then, it is likely that they don't allow individual services to connect directly to the EPP endpoints since the registries impose connection count limits.
