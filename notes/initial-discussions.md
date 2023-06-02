# Initial Discussions

The early ideas of the .country project are already materialized in a published article, [Working with web2 domain systems in web3 ](https://github.com/harmony-one/dot-country/blob/main/Design.md). The article explains many foundational ideas, definitions, and concepts. It is recommended to read the article in full, before reading this document.

The article does not get into details of registry or registrar implementations, but they were discussed privately. This document encapsulates the some initial thoughts in the very early phase of the .country project, including analysis of web3 components and potential open source software that can be used to service the web2 components. However, the plan to operate a registry or registrar was halted due to the strict requirements from ICANN, and the fact that qualification application may take a long period of time to process.

### Registry

Registry maintains domain ownership, expiry, and contact information record. Registry only communicates with registrars. Registry does not manage the DNS records (which is the responsiblity of the nameserver assigned by the domain). Currently, the .country registry operater is delegated to be [Internet Naming Co.](https://internetnaming.co/) (formerly known as Uni Naming & Registry)

## Web3 Registry

We want to operate a registry that manages the TLD, such that we can interface with registrars who will process user-facing registration and renewal requests. The registry-registrar communication interface is in [EPP commands](https://datatracker.ietf.org/doc/html/rfc3731). We evaluated open source registry projects, and found Nomulus to be the best candidate to use if we were to operate our own. Nomulus has built-in tools to parse and process EPP commands.

We need to add web3 capabilities to the registry managed by Nomulus, so it reads and writes the blockchain when it is processing EPP requests (and other types of requests) from registrars. Since existing registrars are not taking the necessary web3 information from users (which at minimum, should include wallet address and a signature to authorize a request), we will also need to build a simple registrar to demonstrate how it should be done. In the non-custodial case, the registrar can be a UI-only client. This should be what we build first If the registrar wants to operates in a conventional way and takes the custody of the domains for users without web3 wallets, it will also need a server. We should build a simple example of that as well, so we can partner with traditional registrars

### Fronted

The [ENS Frontend v3](https://github.com/harmony-one/ens-app-v3) (deployed at <https://ens.demo.harmony.one/>) can be used by users to register and manage their domains. An extra features was implemented to allow the user claiming the equivalent web2 domain after they finish registring the web3 domain. This frontend can be further revised to serve our purposes, or to become a template for web3 registrar.

### Nameserver

Initially, all domains registered through our system should be assigned to a nameserver which we manage, so that we can provide add blockchain-based features at the nameserver, such as to enable users to configure DNS records on-chain. The nameserver will be the authoritative DNS for the domains. At a later stage, we may allow users to choose their own nameservers, or to define an open protocol extension such that other developers can operate their own DNS server. 

Below is our evaluation of existing open source DNS server projects

#### Evaluation criteria

- Built-in authentication systems - we need to transform this to web3 authentication
- Gated APIs for updating records
- Support common DNS record types (A, CNAME, MX, TXT)
- Ideally in modern languages (Golang, Rust, C++17 or later, Python, Node.js, Scala)
- We could consider implementing a full web3 version and store all records on smart contracts (extending ENS contracts or building our own)

#### Summary

A list of potential code bases for DNS Name Servers was collected and a deep dive of the top 6 eligble code bases was conducted. Inital thoughts are that [core-dns](#core-dns-go---ranking-1) is the most eligible based on it's strong adoption, the fact that it's built in go which has good support for the web3 backend and that it has a plugin architecture with a reference plugin for ENS which can be used for inspiration. [trust-dns](#trust-dns-rust---ranking-2) is currently ranked second as it is not as widely adopted, it is written in rust which has proven web3 backend integration, it can be deployed as a standalone instance or customized crates can be developed which can be leveraged by the 1600 repositories which use the trust-dns-server crate today and there exists some ENS rust integrations, though they are minimal and minimally maintained. [nsd](#nsd-c---ranking-3) was placed lower on the list as it is built in C and web3 backend integration is uncertain, also it has a BSD license which is a little stricter than the GNU, MIT and Apache Licensing. That being said it is a mature and well adopted server with extensive documentation and support and the ability to engage NnetLabs professional services to speed up development time.

Some other repositories are included for completeness, but they are not viable to be used as a baseline: [gdnsd](#gdnsd-c---ranking-4) is not actively maintained and is primarily for querying rather than record management. [node-dns](#node-dns-javascript---ranking-4) provides some tools for javascript projects to interact with DNS servers. It doest not have update functionality for DNS records. [solana-dns](#solana-dns-javascript---ranking-5) is included since it is one of the first web3 DNS backend, but the project stayed at a concept stage and was never fully developed.

DNS Server Projects: (see more at <https://github.com/topics/dns-server>)

- [Rust](https://github.com/topics/dns-server?l=rust)
  - <https://github.com/bluejekyll/trust-dns>
  - <https://github.com/Revertron/Alfis> (blockchain in title)
- [Go](https://github.com/topics/dns-server?l=go)
  - <https://github.com/coredns/coredns>
- [C++](https://github.com/topics/dns-server?l=c%2B%2B)
  - <https://github.com/PowerDNS/pdns> (GNU)
- [C](https://github.com/topics/dns-server?l=c)
  - <https://github.com/NLnetLabs/nsd>
  - <https://github.com/gdnsd/gdnsd>
  - <https://github.com/samboy/MaraDNS>
  - <https://github.com/isc-projects/bind9>
  - <https://github.com/CZ-NIC/knot>  [docs](https://www.knot-dns.cz/docs/3.2/html/introduction.html#what-is-knot-dns)
  - <https://github.com/dnomd343/ClearDNS> (Chinese README.md)
- [C#](https://github.com/topics/dns-server?l=c%23)
  - <https://github.com/TechnitiumSoftware/DnsServer> (C#)
  - <https://github.com/fanliang11/surging>
- [Javascript](https://github.com/topics/dns-server?l=javascript)
  - <https://github.com/song940/node-dns>
  - <https://github.com/Monadical-SAS/solana-dns>
- [Python](https://github.com/topics/dns-server?l=python)
  - <https://github.com/ctxis/SnitchDNS>

</details>

#### core-dns (go) - Ranking 1

**Summary**

[core-dns](https://github.com/coredns/coredns) is a popular repository with over 10,000 stars and 1800 forks. It has a comprehensive range of plugins<sup>[1](#cd1)</sup><sup>[2](#cd2)</sup> and it allows the chaining of plugins to customize DNS resolution in response to a client request. A list of essential plugins (a.k.a core plugins) are developed by CoreDNS team<sup>[3](#cd3)</sup>. Many other popular plugins are  developed by third party developers in the community<sup>[4](#cd4)</sup>

In particular, the ENS Plugin<sup>[5](#cd5)</sup> enables nameservers to retrieve information from the Ethereum Blockchain seamlessly with the existing infrastructure. This plugin uses go-ens<sup>[6](#cd6)</sup> to interact with the ens-contracts.<sup>[7](#cd7)</sup> All repositories appear to be actively maintained with recent commits.

We may develop a Harmony specific plugin for CoreDNS.

**Evaluation Criteria**

1. Authentication: Is currently supported by the acl-plugin<sup>[7](#cd7)</sup> which can optionally be enabled to `users are able to block or filter suspicious DNS queries by configuring IP filter rule sets, i.e. allowing authorized queries or blocking unauthorized queries.` A similar plugin could be written to enable authentication via web3.
2. APIs: Can be enabled by plugin
3. DNS Record Type Support: Yes
4. Modern Language: Yes
5. Development Timeframe: Although the CoreDNS framework is large, its architecture allows constructing a specific plugin without reviewing the entire codebase. The existing ENS plugin is also helpful in reducing development time.

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

#### trust-dns (rust) - Ranking 2

**Summary**

[trust-dns](https://github.com/bluejekyll/trust-dns): is a widely used project with 2,600 stars, 313 forks and 1077 users of the project. It is built using rust and has a modular framwork producing 8 rust crates<sup>[1](#td1)</sup> which can be used by other rust applications. It can also be deployed as a standalone DNS authoritative server. The project focuses on<sup>[2](#td2)</sup> safety, security and simplicity. It has widespread support for Internet Engineering Task Force (IETF) Request for Comments (RFC's)<sup>[3](#td3)</sup>.

**Development Approach**

We may deploy a standalone version and modify the [server crate](https://github.com/bluejekyll/trust-dns/tree/main/crates/server)<sup>[1](#td1)</sup>, to use Harmony Blockchain instead of sql-lite for [store](https://github.com/bluejekyll/trust-dns/tree/main/crates/server/src/store) functionality. Note: there are minimal ENS rust codebases<sup>[4](#td4)</sup><sup>[5](#td5)</sup><sup>[6](#td6)</sup> which can be leveraged to develop a Harmony ENS backend. A bonus benefit is if we customize the server and publish crate, it could potentially be used by the 1684 projects currently using trust-dns-server<sup>[7](#td7)</sup>.

**Evaluation Criteria**

1. Authentication: Unknown
2. APIs: Unknown
3. DNS Record Type Support: Good
4. Modern Language: Yes, Rust
5. Development Timeframe: The development approach is fairly clear. Factors to consider is the availibility of Rust developers and the fact that there are minimal ENS-related implementations to leverage.

**References**

<a name="td1">[1]</a>  [Trust-DNS Crates](https://trust-dns.org/): Rust crates developed by Trust-DNS

- [Trust-DNS](https://crates.io/crates/trust-dns): Binaries for running a DNS authoritative server.
- [Proto](https://crates.io/crates/trust-dns-proto): trust-dns-proto Raw DNS library, exposes an unstable API and only for use by the other Trust-DNS libraries, not intended for end-user use.
- [Client](https://crates.io/crates/trust-dns-client): trust-dns-client Used for sending query, update, and notify messages directly to a DNS server.
- [Server](https://crates.io/crates/trust-dns-server): trust-dns-server Use to host DNS records, this also has a named binary for running in a daemon form.
- [Resolver](https://crates.io/crates/trust-dns-resolver): trust-dns-resolver Utilizes the client library to perform DNS resolution. Can be used in place of the standard OS resolution facilities.
- [Rustls](https://crates.io/crates/trust_dns_rustls): trust-dns-rustls Implementation of DNS over TLS protocol using the rustls and ring libraries.
- [NativeTls](https://crates.io/crates/trust_dns_native_tls): trust-dns-native-tls Implementation of DNS over TLS protocol using the Host OS’ provided default TLS libraries
- [OpenSsl](https://crates.io/crates/trust_dns_openssl): trust-dns-openssl Implementation of DNS over TLS protocol using OpenSSL

<a name="td2">[2]</a>  [Trust-DNS Goals](https://github.com/bluejekyll/trust-dns#goals):

- Build a safe and secure DNS server and client with modern features.
- No panics, all code is guarded
- Use only safe Rust, and avoid all panics with proper Error handling
- Use only stable Rust
- Protect against DDOS attacks (to a degree)
- Support options for Global Load Balancing functions
- Make it dead simple to operate

<a name="td3">[3]</a>  [Trust-DNS RFCs implemented](https://github.com/bluejekyll/trust-dns/blob/main/README.md#rfcs-implemented): A list of RFCs implemented and in progress by trust-dns.

<a name="td4">[4]</a>  [rust ens-rainbow](https://github.com/graphprotocol/ens-rainbow): Developed by graphprotocol in 2019 is an upload utility for ens_names.sql.gz

<a name="td5">[5]</a>  [rust-ens crate](https://crates.io/crates/ens): Used by ens-rainbos it is a Rust ENS(Ethereum Name Service) interface, based on rust-web3.

<a name="td6">[6]</a>  [rust ens-api](https://github.com/EnormousCloud/ens-api): Ethereum Name Service RESTful API developed and last modified in December 2021.

<a name="td7">[7]</a>  [trust-dns-server Dependency graph](https://github.com/bluejekyll/trust-dns/network/dependents?dependents_after=MjMwODY3NDk4MDM): Repositories that depend on trust-dns-server.

#### nsd (C) - Ranking 3

**Summary**

[nsd](https://github.com/NLnetLabs/nsd) is developed by NLNETLABS<sup>[1](#ns1)</sup> and is licensed under BSD 3<sup>[1](#ns1)</sup>.  It has 302 stars, 82 forks and 29 contributors and is actively maintained. They have comprehensive documentation<sup>[3](#ns3)</sup>, robust RFC compliance<sup>[4](#ns4)</sup> and offer proffesional services including training, development and support<sup>[5](#ns5)</sup>.

**Development Approach**

Modify the [server](https://github.com/NLnetLabs/nsd/blob/master/server.c) and [storage](https://github.com/NLnetLabs/nsd/blob/master/server.c) capabilities. If needed, we could engage NLNetLabs consultancy team. Note the integration complexity of the web3 backend (solidity) and C needs to be investigated further. Existing projects such as cpp-ethereum<sup>[6](#ns1)</sup> and aleth<sup>[7](#ns1)</sup> are already archived.

**Evaluation Criteria**

1. Authentication: Robust
2. APIs: Robust
3. DNS Record Type Support: Robust
4. Modern Language: Yes
5. Development Timeframe: Highly dependent on the integration of solidity and C.

**References**

<a name="ns1">[1]</a>  [NLNETLABS NSD](https://nlnetlabs.nl/projects/nsd/about/): The NLnet Labs Name Server Daemon (NSD) is an authoritative DNS name server. It has been developed for operations in environments where speed, reliability, stability and security are of high importance.

<a name="ns2">[2]</a>  [BSD3-Clause](https://choosealicense.com/licenses/bsd-3-clause/): A permissive license similar to the BSD 2-Clause License, but with a 3rd clause that prohibits others from using the name of the copyright holder or its contributors to promote derived products without written consent.

<a name="ns3">[3]</a>  [NSD user guide](https://nsd.docs.nlnetlabs.nl/en/latest/): documentation of the NLnet Labs Name Server Daemon (NSD)

<a name="ns4">[4]</a>  [NSD RFC Compliance](https://nlnetlabs.nl/projects/nsd/rfc-compliance/): NLnet Labs has a long history of supporting an Open Internet and Open Standards. NSD strives to be a reference implementation for emerging standards in the Internet Engineering Task Force (IETF).

<a name="ns5">[5]</a>  [NSD Support](https://nlnetlabs.nl/projects/nsd/support/): NSD offers both community level support and Proffesional Services including [support contracts](https://nlnetlabs.nl/services/contracts/) and [consultancy](https://nlnetlabs.nl/services/consultancy/) and training.

<a name="ns6">[6]</a>  [cpp-ethereum](https://github.com/ethereumproject/cpp-ethereum): cpp-ethereum - Ethereum C++ client (Ethereum Classic Blockchain) (This repository has been archived by the owner before Nov 8, 2022. It is now read-only.)

<a name="ns7">[7]</a>  [aleth ethereum](https://github.com/ethereum/aleth): Ethereum C++ client, tools and libraries (This project has been discontinued and is no longer maintained.)

#### gdnsd (C) - Ranking 5

**Summary** 

[gdnsd](https://github.com/gdnsd/gdnsd) is an Authoritative-only DNS server. The initial g stands for Geographic, as gdnsd offers a plugin system for geographic (or other sorts of) balancing, redirection, and service-state-conscious failover. It is developed by gdnsd.org<sup>[1](#gd1)</sup> and has 9 contributors. The repository has 416 stars, and 65 forks. It is used by<sup>[1](#gd1)</sup> prominent organizations such as Wikimedia foundation and Logitech. The project is licensed under GNU General Public License. It does not appear to be actively maintained. The last commits were on Aug 13, 2021 and Dec 5, 2022 (over a year old). The system does support a plugin framework<sup>[2](#gd2)</sup>.

**Development Approach**

Server, storage, and RFC compliance will require more work. The development is also highly dependent on web3 backend (solidity) and C integration.

**Evaluation Criteria**

1. Authentication: Limited
2. APIs: Limited
3. DNS Record Type Support: Query only support
4. Modern Language: Yes
5. Development Timeframe: Long

**References**

<a name="gd1">[1]</a> [gdnsd.org](https://gdnsd.org/): Website including overview, news, resources, users, licensing and history.

<a name="gd2">[2]</a> [gdnsd wiki](https://github.com/gdnsd/gdnsd/wiki): Wiki including plugin docs automatically exported from the main source tree's pod sources.

#### node-dns (javascript) - Ranking 5

**Summary**

[node-dns](https://github.com/song940/node-dns) is a DNS Server and client implementation in pure javascript with no dependencies. It has 395 stars, 54 forks, and is used by 274 repos<sup>[1](#nd1)</sup>. The project has 15 contributors and 4294 weekly downloads<sup>[2](#nd2)</sup>. This tool would work in conjunction with an existing DNS. It would be most useful for javascript projects looking to interact with DNS servers, but offers no ability to store DNS records itself. The project only support query related RFCs<sup>[3](#nd3)</sup>

**Developement Approach** 

Use this for DNS integrations.

**Evaluation Criteria**

1. Authentication: N/A
2. APIs: N/A
3. DNS Record Type Support: N/A
4. Modern Language: N/A
5. Development Timeframe: N/A

**References**

<a name="nd1">[1]</a>  [node-dns Dependency graph](https://github.com/song940/node-dns/network/dependents): Repositories that depend on dns2

<a name="nd2">[2]</a>  [dns2 npm package](https://www.npmjs.com/package/dns2): A DNS Server and Client Implementation in Pure JavaScript with no dependencies. (4,294 weekly downloads as at Dec 5th, 2022).

<a name="nd3">[3]</a>  [node-dns relevant specifications](https://github.com/song940/node-dns#relevant-specifications): support for Internet Engineering Task Force (IETF) Request for Comments (RFC's).

#### solana-dns (javascript) - Ranking 5

**Summary**

[solana-dns](https://github.com/Monadical-SAS/solana-dns) was built as a conceptual prototype for Solana about 3 years ago by Nick Sweeting<sup>[1](#sd1)</sup>. It was never completed, but has some interesting ideas<sup>[2](#sd2)</sup> to take inspiration from.

**Evaluation Criteria**

1. Authentication: No
2. APIs: N/A
3. DNS Record Type Support: N/A
4. Modern Language: Yes, javascript
5. Development Timeframe: Very long, require building from scratch without much code to leverage.

**References**

<a name="sd1">[1]</a> [Nick Sweeting Twitter](https://twitter.com/thesquashSH): Reached out to Nick regarding the status of the project and he responded with
> [The Project] Just stopped, it was a fun side project proposal to our friends on the solana team for a potential use case for the platform. They ended up giving us other solana work instead but thought dns was a cool idea, never got funding to do the DNS thing though. Feel free to rip off all/some/any of it, it's up for grabs if you want to build something similar! 🙂

<a name="sd2">[2]</a> [How does DNS work normally?](https://github.com/Monadical-SAS/solana-dns#how-does-dns-work-normally): The repository's README.md gives an overview of the approach and potential problems. It also contains a [Data Flow](https://github.com/Monadical-SAS/solana-dns#data-flow) and [Execution Flow](https://github.com/Monadical-SAS/solana-dns#execution-flow).

## Related Standards

- [RFC 8499](https://tools.ietf.org/html/rfc8499): No more master/slave, in honor of [Juneteenth](https://en.wikipedia.org/wiki/Juneteenth)

### Basic operations

- [RFC 1035](https://tools.ietf.org/html/rfc1035): Base DNS spec (see the Resolver for caching)
- [RFC 2308](https://tools.ietf.org/html/rfc2308): Negative Caching of DNS Queries (see the Resolver)
- [RFC 2317](https://tools.ietf.org/html/rfc2317): Classless IN-ADDR.ARPA delegation
- [RFC 2782](https://tools.ietf.org/html/rfc2782): Service location
- [RFC 3596](https://tools.ietf.org/html/rfc3596): IPv6
- [RFC 6891](https://tools.ietf.org/html/rfc6891): Extension Mechanisms for DNS
- [RFC 6761](https://tools.ietf.org/html/rfc6761): Special-Use Domain Names (resolver)
- [RFC 6762](https://tools.ietf.org/html/rfc6762): mDNS Multicast DNS (experimental feature: `mdns`)
- [RFC 6763](https://tools.ietf.org/html/rfc6763): DNS-SD Service Discovery (experimental feature: `mdns`)
- [RFC ANAME](https://tools.ietf.org/html/draft-ietf-dnsop-aname-02): Address-specific DNS aliases (`ANAME`)

### Update operations

- [RFC 1995](https://tools.ietf.org/html/rfc1995): Incremental Zone Transfer
- [RFC 1996](https://tools.ietf.org/html/rfc1996): Notify secondaries of update
- [RFC 2135](https://www.rfc-editor.org/rfc/rfc2136): Dynamic Updates in the Domain Name System (DNS UPDATE)
- [RFC 2136](https://tools.ietf.org/html/rfc2136): Dynamic Update
- [RFC 7477](https://tools.ietf.org/html/rfc7477): Child-to-Parent Synchronization in DNS
- [Update Leases](https://tools.ietf.org/html/draft-sekar-dns-ul-01): Dynamic DNS Update Leases
- [Long-Lived Queries](https://tools.ietf.org/html/draft-sekar-dns-llq-01): Notify with bells

### Secure DNS operations

- [RFC 3007](https://tools.ietf.org/html/rfc3007): Secure Dynamic Update
- [RFC 4034](https://tools.ietf.org/html/rfc4034): DNSSEC Resource Records
- [RFC 4035](https://tools.ietf.org/html/rfc4035): Protocol Modifications for DNSSEC
- [RFC 4509](https://tools.ietf.org/html/rfc4509): SHA-256 in DNSSEC Delegation Signer
- [RFC 5155](https://tools.ietf.org/html/rfc5155): DNSSEC Hashed Authenticated Denial of Existence
- [RFC 5702](https://tools.ietf.org/html/rfc5702): SHA-2 Algorithms with RSA in DNSKEY and RRSIG for DNSSEC
- [RFC 6844](https://tools.ietf.org/html/rfc6844): DNS Certification Authority Authorization (CAA) Resource Record
- [RFC 6698](https://tools.ietf.org/html/rfc6698): The DNS-Based Authentication of Named Entities (DANE) Transport Layer Security (TLS) Protocol: TLSA
- [RFC 6840](https://tools.ietf.org/html/rfc6840): Clarifications and Implementation Notes for DNSSEC
- [RFC 6844](https://tools.ietf.org/html/rfc6844): DNS Certification Authority Authorization Resource Record
- [RFC 6944](https://tools.ietf.org/html/rfc6944): DNSKEY Algorithm Implementation Status
- [RFC 6975](https://tools.ietf.org/html/rfc6975): Signaling Cryptographic Algorithm Understanding
- [RFC 7858](https://tools.ietf.org/html/rfc7858): DNS over TLS (feature: `dns-over-rustls`, `dns-over-native-tls`, or `dns-over-openssl`)
- [RFC DoH](https://tools.ietf.org/html/draft-ietf-doh-dns-over-https-14): DNS over HTTPS, DoH (feature: `dns-over-https-rustls`)
- [DNSCrypt](https://dnscrypt.org): Trusted DNS queries
- [S/MIME](https://tools.ietf.org/html/draft-ietf-dane-smime-09): Domain Names For S/MIME

