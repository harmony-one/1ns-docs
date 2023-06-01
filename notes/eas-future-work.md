# Self-hosted Mail Server

## Overview

Here we provide an analysis for existing open source SMTP and mail-fowarding services, and a list of engineering tasks for integrating such service and replacing ImprovMX in EAS.

After an evaluation of existing open source SMTP implementations, we decided to use Maddy[^71] due to its modular framework and support of web3 plugins. The analysis can be found in [Appendix A -SMTP Server Review](#appendix-a---smtp-server-review).


## Technical Details

Based on [Email Alias Service](./eas.md), when the user activates the service, a smart contract commitment is made on-chain specific to the alias and the forwarding address. Afterwards, an API call is made to EAS server, where the server verifies the commitment and subsequently works with the mail service provider (ImprovMX) to establish the mail forwarding. 

In lieu of ImprovMX, the EAS server could make use of Maddy's envelope sender and recipient rewriting module (^74) to setup the forwarding configuration. In the current flow, the EAS server is also responsible for setting up the MX and TXT (for SPF) record of the domain. This can be done in the same fashion it is done right now (granting the server special access to write to the Redis database where the domain records are kept), or simply via frontend interactions, when blockchain DNS is deployed (go-1ns[^10]). After MX record is set, Maddy SMTP Pipeline [^73] will handle all incoming emails to the aliases for the domain, and forward the emails to configured recipients accordingly. 

The same Maddy mail server can also handle sending emails as a vanilla SMTP server. 

## TODOs

### Approach 1: Extend Maddy with admin APIs

The admin APIs will allow authenticated callers to add, update, and delete SMTP domains and mail-forwarding configurations. EAS server will be required to integrate with these APIs in lieu of ImprovMX.

### Approach 2: Standalone admin server for SMTP
 
Instead of managing Maddy configuration as a plugin, the standalone server could manage the configuration externally (e.g. from file system or databases). The server could have more flexibility in terms of authentication and special access (e.g. being granted with special permission to access DNS settings, integrated with go-1ns). Moreover it may implement caching and distributed services, thereby being able to handle large number of user requests better than a plugin. It could also support more versatile authentications (such as via wallet signatures) and allow users to lookup or manage some settings directly

Overall, we favor approach 1 since it is simpler. There is no concrete need for approach 2 at this time, but this may change later.

## Admin APIs

1. Domains: manage domains based on user account (can be wallet address)
3. Aliases: manage aliases for a given domain (list, add, bulk add, update, delete).
4. Logs: retrieve message logs for domains and aliases.
5. SMTP: manage SMTP credentials (list, add, update, delete).

## Appendices

### Appendix A - Open Source SMTP Mail Servers

Below [open source smtp github repositories](https://github.com/search?o=desc&q=smtp&s=forks&type=Repositories) were identified and evaluated, based on the following selection criteria:

1. Language: Go, Node are preferred
2. Adoption: number of forks and quality of documentation)
3. Plugin capability: ability to add a plugin or modify the code modularly to integrate with web3 backend and optionally decentralized storage)
4. Extensibility capabilities: to create admin APIs

Repositories:

* [GoLang](https://go.dev/#) [search](https://github.com/search?l=Go&o=desc&q=smtp&s=forks&type=Repositories)
  * [Maddy Mail Server](https://github.com/foxcpp/maddy): Maddy Mail Server implements all functionality required to run a e-mail server. It supports [modules](https://maddy.email/reference/modules/) this is the [Issue capturing Modular Design](https://github.com/foxcpp/maddy/issues/15) and [here are the go docs](https://pkg.go.dev/github.com/foxcpp/maddy@v0.6.2#section-readme). Here is an article[Self-Hosted Email With maddy: A Naive First Attempt](https://mediocregopher.com/posts/selfhosted-email-with-maddy).
  * [go-smtp](https://github.com/emersion/go-smtp)
  * [mail-cow](https://github.com/mailcow/mailcow-dockerized): The integrated mailcow UI allows administrative work on your mail server instance as well as separated domain administrator and mailbox user access.
  * [MailHog](https://github.com/mailhog/MailHog): MailHog is an email testing tool for developers. [smtp](https://github.com/mailhog/smtp)
  * [go-guerilla](https://github.com/flashmob/go-guerrilla)
* [Nodejs](https://nodejs.org/en/) [search](https://github.com/search?l=JavaScript&o=desc&q=smtp&s=stars&type=Repositories)
  * [Haraka](https://github.com/haraka/Haraka): Haraka is a highly scalable node.js email server with a modular plugin architecture. ([configuration](https://github.com/haraka/Haraka#configure-haraka), [plugins](https://github.com/haraka/Haraka/blob/master/config/plugins))
  * [nodemailer smtp-server](https://github.com/nodemailer/smtp-server): Nodemailer is a module for Node.js applications  [docs](https://nodemailer.com/extras/smtp-server/)
* [C, c++](https://en.wikipedia.org/wiki/C_(programming_language)) (searches [C](https://github.com/search?l=C&o=desc&q=smtp&s=stars&type=Repositories), [C#](https://github.com/search?l=C%23&o=desc&q=smtp&s=stars&type=Repositories), [C++](https://github.com/search?l=C%2B%2B&o=desc&q=smtp&s=stars&type=Repositories) )
  * [https://github.com/hmailserver/hmailserver](https://github.com/hmailserver/hmailserver)
* [Python](https://www.python.org/)
  * [https://github.com/modoboa](https://github.com/modoboa)
* others
  * [https://github.com/iredmail/iRedMail](https://github.com/iredmail/iRedMail)


## Footnotes

[^5]: [Project .country](https://github.com/harmony-one/dot-country/blob/main/README.md): the DC contract is meant to provide some "extra service" on top of ENS contracts. Therefore, it may charge some additional fees on top of purchasing an ENS domain from the ENS app. (github)

[^9]: [ens-deployer](https://github.com/harmony-one/ens-deployer): 1NS related contract deployment, customized based on ENS contracts.

[^10]: [go-1ns](https://github.com/jw-1ns/go-1ns): Go module to simplify interacting with the 1 Name Service contracts. Initial version copied from go-ens

[^61]: [ImprovMX](https://improvmx.com/api/): The [Improvmx](https://improvmx.com/) hosted smtp server api's provide a good reference point for the DEES web3 implementation.

[^71]: [Maddy Mail Server](https://github.com/foxcpp/maddy): Maddy Mail Server implements all functionality required to run a e-mail server. [Here are the go docs](https://pkg.go.dev/github.com/foxcpp/maddy@v0.6.2#section-readme). Here is an article[Self-Hosted Email With maddy: A Naive First Attempt](https://mediocregopher.com/posts/selfhosted-email-with-maddy). [Here is a comparison with MailCow](https://maddy.email/faq/#how-maddy-compares-to-mailcow-or-mail-in-the-box).

[^72]: [Maddy Modular Design](https://maddy.email/reference/modules/): maddy is built of many small components called "modules". Each module does one certain well-defined task.  This is the [Issue capturing Modular Design](https://github.com/foxcpp/maddy/issues/15).

[^73]: [Maddy SMTP message routing pipeline](https://maddy.email/reference/smtp-pipeline/): Message pipeline is a set of module references and associated rules that describe how to handle messages.

[^74]: [Maddy Envelope sender / recipient rewriting](https://maddy.email/reference/modifiers/envelope/): `replace_sender` and `replace_rcpt` modules replace SMTP envelope addresses based on the mapping defined by the table module (maddy-tables(5)). It is possible to specify 1:N mappings. This allows, for example, implementing mailing lists.