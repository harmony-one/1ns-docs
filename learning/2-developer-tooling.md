# Developer Tooling (20 mins)

## Overview

1. Contribution Guides (5 mins)
2. Development Tools (5 mins)
3. Programming Languages (5 mins)
4. Machine Setup (5 mins)

## Contributon Guides

- [Engineering Guide for Harmony Protocol](https://github.com/harmony-one/Engineering-Guide)
- [Harmony Contribution Guide](https://github.com/harmony-one/harmony/blob/main/CONTRIBUTING.md) (old version needs to be updated)

## Development Tools

- Integrated Development Environment: [Webstorm](https://www.jetbrains.com/webstorm/) and [Visual Studio](https://visualstudio.microsoft.com/) are commonly used with additional [extensions](https://marketplace.visualstudio.com/) and [plugins](https://plugins.jetbrains.com/webstorm) for programing languages based on developer needs. Alternately you can use [vim](https://www.vim.org/).
- Version Management: [github](https://github.com/) is used for version managemt
- Command Line: [iterm2](https://iterm2.com/) offers robust terminal features.

## Programming Languages

- [javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [typescript](https://www.typescriptlang.org/)
- [solidity](https://soliditylang.org/)
- [golang](https://go.dev/):

## Machine Setup (mac)

### Development Tools (everyone)

- [github](https://github.com/) : download [github cli](https://github.com/cli/cli#installation): and authenticate using [gh auth login](https://cli.github.com/manual/gh_auth_login)
- [vim] : install [The Ultimate Vim configuration](https://github.com/amix/vimrc)
- [terminal] : download [iterm2](https://iterm2.com/)
- ide : [Visual Studio](https://visualstudio.microsoft.com/vs/mac/)
- [chrome](https://www.google.com/chrome/) : download [chrome](https://www.google.com/chrome/dr/download/) chrome
- [metamask](https://www.google.com/chrome/): download [metamask](https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn) and set up a wallet. *Note if managaing real funds a hardware wallet such as [ledger](https://www.ledger.com/) is recommended if so also install [ledger live](https://www.ledger.com/ledger-live).*

Optional

- [docker](https://www.docker.com/): download [docker](https://www.docker.com/) if doing local devleopment for complex projects using multiple components locally. See [ens-app-v3](https://github.com/harmony-one/ens-app-v3/blob/dev/docs/Developer.md) for a sample project using docker to deploy multiple components locally.

### Javascript Development

- [node](https://nodejs.org/en/about): download [node](https://nodejs.org/en)
- [nvm](https://github.com/nvm-sh/nvm): install [nvm](https://github.com/nvm-sh/nvm#installing-and-updating)
- [yarn](https://yarnpkg.com/): install [yarn](https://yarnpkg.com/getting-started/install)
- [pnpm](https://pnpm.io/installation): `npm install -g pnpm`
- [homebrew](https://brew.sh/): install [homebrew](https://brew.sh/)
- [typescript](https://www.typescriptlang.org/): install npm package [ts-loader](https://www.npmjs.com/package/ts-loader) globally and install [typescript](https://www.typescriptlang.org/download) in development projects as needed.
- [eslint](https://eslint.org/): install [eslint](https://www.npmjs.com/package/eslint) globally
-

### Solidity Devlopment

- [ganache](https://github.com/trufflesuite/ganache):  install [ganache](https://www.npmjs.com/package/ganache) globally
- [hardhat](https://hardhat.org/): install [hardhat](https://hardhat.org/hardhat-runner/docs/getting-started#installation) in development projects as needed.
- [hardhat plugins](https://hardhat.org/hardhat-runner/plugins): install [hardhat plugins](https://hardhat.org/hardhat-runner/plugins) in development projects as needed. [Here](https://github.com/johnwhitton/bc_template/blob/main/docs/README.md) is a sample project which utilizes many of the plugins.

### Golang Development

- [go](https://go.dev/): [Install go](https://go.dev/dl/)

## Full Stack

The following are needed for full stack local development e.g [ens-app-v3](https://github.com/harmony-one/ens-app-v3) which uses [thegraph](https://thegraph.com/docs/en/)

- Rust (latest stable) – [How to install Rust](https://www.rust-lang.org/en-US/install.html)
  - Note that `rustfmt`, which is part of the default Rust installation, is a build-time requirement.
- PostgreSQL – [PostgreSQL Downloads](https://www.postgresql.org/download/)
- IPFS – [Installing IPFS](https://docs.ipfs.io/install/) ([mac](https://docs.ipfs.tech/install/ipfs-desktop/#macos))
- Profobuf Compiler - [Installing Protobuf](https://grpc.io/docs/protoc-installation/)
- [graph](https://github.com/graphprotocol/graph-node/blob/master/README.md)

Sample installation commands

```bash
# rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# PostgresSQL
brew upgrade
brew install postgresql@15
echo 'export PATH="/usr/local/opt/postgresql@15/bin:$PATH"' >> /Users/johnwhitton/.bash_profile
source ~/.bash_profile

# Optionally start as a service (runs postgres on restart)
#brew services start postgresql@15
#brew services stop postgresql@15
# brew services run postgresql

# run postgres
# /usr/local/opt/postgresql\@15/bin/postgres -D /usr/local/var/postgresql\@15/

# IPFS https://docs.ipfs.tech/install/ipfs-desktop/#package-managers
#brew install ipfs --cask

# ipfs https://docs.ipfs.tech/install/command-line/#install-ipfs-kubo
curl -O https://dist.ipfs.tech/kubo/v0.19.0/kubo_v0.19.0_darwin-amd64.tar.gz
# Apple silicon curl -O https://dist.ipfs.tech/kubo/v0.19.0/kubo_v0.19.0_darwin-arm64.tar.gz
tar -xvzf kubo_v0.19.0_darwin-amd64.tar.gz
cd kubo
sudo bash install.sh




# Protobuf https://grpc.io/docs/protoc-installation/
brew install protobuf
protoc --version  # Ensure compiler version is 3+
```

## Configuring your development environment

### Screen usage

It is often useful to have multiple windows available as a developer. This can include using multiple monitors and/or using [split screens](https://support.apple.com/en-us/HT204948). Also if wanting to scroll through windows one can use `Command-Tab` to switch between open application.

A typical development environment may use 4 screens

- Main (split screen)
  - chrome: research
  - iterm2: devops
- Screen1 : IDE
- Screen2: Communication

### Configuring your Bash Profile

You can configure your bash profile to simplify tasks such as git commands, changing to directories, defaulting node versions and more. See [Appendix A](#apendix-a-sample-bash-profile) for an example `.bash_profile`.

### Integrated Development Environment

Your IDE can be customized for different development languages and linting. Below are some extensions and configuration for the [visual studio ide](https://visualstudio.microsoft.com/).

#### [Visual Studio Extensions](https://marketplace.visualstudio.com/)

Following are some extensions which developers may find useful for visual studio

- [vim](https://marketplace.visualstudio.com/): VSCodeVim is a Vim emulator for Visual Studio Code.
- [docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker): The Docker extension makes it easy to build, manage, and deploy containerized applications from Visual Studio Code.
- [DotEnv](https://marketplace.visualstudio.com/items?itemName=mikestead.dotenv): VSCode .env syntax highlighting
- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint): Integrates [ESLint](https://eslint.org/) into VS Code. If you are new to ESLint check the [documentation](https://eslint.org/).
- [Github Pull Request and Issues](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github): Review and manage your GitHub pull requests and issues directly in VS Code
- [Github Theme](https://marketplace.visualstudio.com/items?itemName=GitHub.github-vscode-theme): Enables the look and feel of github themes in visual studio.
- [go](https://marketplace.visualstudio.com/items?itemName=golang.Go): The VS Code Go extension provides rich language support for the Go programming language.
- [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one): Supports Markdown keyboard shortcuts, table of contents, auto preview and more.
- [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint): Markdown/CommonMark linting and style checking for Visual Studio Code
- [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json): Prettify ugly JSON inside VSCode
- [Solidity](https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity): Solidity support including syntaxk highlighting, linting, code completion, verson support, compilation and more.

#### Visual Studio Configuration

Following is a sample configuration file which formats on save and enables solidity navigation to definitions.

```json
{
  "security.workspace.trust.untrustedFiles": "open",
  "editor.formatOnSave": true,
  "solidity.packageDefaultDependenciesDirectory": "contract/node_modules",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "eslint.validate": [
    "javascript"
  ],
  "eslint.format.enable": true,
  "workbench.colorTheme": "GitHub Dark",
  "window.zoomLevel": -1
}
```

### Linting Configuration

Following is a list of configuration files used for linting in a sample repository.

- <https://github.com/harmony-one/ens-deployer>
  - [.eslintignore](https://github.com/harmony-one/ens-deployer/blob/main/.eslintignore)
  - [.eslintrc.js](https://github.com/harmony-one/ens-deployer/blob/main/contract/.eslintrc.js)
  - [.npmignore](https://github.com/harmony-one/ens-deployer/blob/main/contract/.npmignore)
  - [.solhint.json](https://github.com/harmony-one/ens-deployer/blob/main/contract/.solhint.json)
  - [.solhint.ignore](https://github.com/harmony-one/ens-deployer/blob/main/contract/.solhintignore)

## Appendices

### Apendix A: example bash profile

```bash
export PATH="/usr/local/bin"
export PATH="$PATH:/usr/bin:/bin"
export PATH="$PATH:/usr/sbin:/sbin"
export PATH="$PATH:~/go/bin"
export PATH="$PATH:/usr/local/go/bin"
export PATH="$PATH:/usr/local/bin/helm"
export PATH="$PATH:~/go/bin"
export PATH="$PATH:~/Library/TeXworks"
export PATH="$PATH:/Library/TeX/texbin"
export PATH="$PATH:~/bin"
export PATH="$PATH:~/Library/Python/3.9/bin"
export PATH="$PATH:~/projects/protobuf/protoc_plugin/bin"
export PATH="$PATH:/Applications/SageMath-9.0.app/Contents/Resources/sage"
export PATH="$PATH:~/.cargo/bin/sccache"
export PATH="$PATH:~/flutter/bin"
## Android SDK set up
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools

# Setting :PATH for Python 3.10
# PATH="/Library/Frameworks/Python.framework/Versions/3.10/bin:${PATH}"
# The original version is saved in .bash_profile.pysave
export PATH="$PATH:/usr/local/opt/python/libexec/bin"

# Show branch at bash prompt
parse_git_branch() {
      git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
export PS1="\[\e[32m\]\W \[\e[91m\]\$(parse_git_branch)\[\e[00m\]$ "

# GO configuration
export GOPATH=$HOME/go
export GO111MODULE=on

# NVM config
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

# Rust configuration
export OPENSSL_STATIC=yes
export PATH="/usr/local/opt/openssl@1.1/bin:$PATH"
# if [ -e ~/.nix-profile/etc/profile.d/nix.sh ]; then . ~/.nix-profile/etc/profile.d/nix.sh; fi # added by Nix installer

# The next line updates PATH for the Google Cloud SDK.
if [ -f '~/google-cloud-sdk/path.bash.inc' ]; then . '~/google-cloud-sdk/path.bash.inc'; fi

# The next line enables shell command completion for gcloud.
if [ -f '~/google-cloud-sdk/completion.bash.inc' ]; then . '~/google-cloud-sdk/completion.bash.inc'; fi
# Allow alias expansion

# using pyenv for python versioning
export PATH="/Users/username/.pyenv/shims:${PATH}"
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init --path)"
fi
#alias python=/usr/local/bin/python3.11

# Handy aliases https://www.cyberciti.biz/tips/bash-aliases-mac-centos-linux-unix.html

alias ll="ls -l"
alias cdop="cd ~/projects/; pwd"
alias sb="source ~/.bash_profile"
alias chrome="open -a 'Google Chrome'"
alias sj=" sage -n jupyter"
alias npr="npm run prod"
alias ws="python3 -m http.server 7800"

# Added by install_latest_perl_osx.pl
[ -r ~/.bashrc ] && source ~/.bashrc

# git commands
alias gitll='git log --pretty=oneline'
alias ga="git add ."
alias gb="git branch -a"
alias gcm="git commit -m"
alias gm="git checkout master"
alias gd="git diff"
alias gll="git log --pretty=oneline"
alias gfu="git fetch upstream"
alias gp="git push"
alias gs="git status"
alias grv="git remote -v"
alias gmu="git checkout master; git fetch upstream; git merge upstream/master; git status"

# Git commands
# For initializing submodules need to use ssh
alias gitssh="git config --global --unset-all url.https://github.com/.insteadof;git config --global --unset-all url.https://.insteadof;git config --global url."git@github.com:".insteadOf https://github.com/;git config --global url."git://".insteadOf https://"
# For patch overrides in cargo need to use https
alias githttps="git config --global --unset-all url.git@github.com:.insteadof;git config --global --unset-all url.git://.insteadof;git config --global url."https://github.com/".insteadOf git@github.com:;git config --global url."https://".insteadOf git://"

# yarn commands
alias yb="yarn build"
alias ydht="yarn harmony-testnet:deploy"
alias ydhtr="yarn harmony-testnet:reset"
alias yi="yarn"
alias yt="yarn test"
alias yv="yarn --version"

# flutter
alias fr="flutter clean; flutter pub get; flutter run"

alias gf='grep -ril'

#Ganache
alias ganachem='ganache -m "test test test test test test test test test test test junk"'

# Me
alias cm="cd ~/me; pwd"
alias cmc="cd ~/me/code; pwd"
alias cmk="cd ~/me/kb; pwd"
alias cmr="cd ~/me/research; pwd"
alias cmrw="cd ~/me/research-web; pwd"
alias cmw="cd ~/me/web/johnwhitton-com.github.io; pwd"
alias cmwi="cd ~/me/web/isolab.gg; pwd"

# admin
alias ca="cd ~/admin; pwd"
alias cah="cd ~/admin/harmony; pwd"
alias co="cd ~/admin/harmony/one-wallet/opendev/johnwhitton; pwd"

# sms-wallet aliases

# twilio aliases
alias tmb='twilio api:core:messages:create --from "+14158401410" --to "+17372327333" --body "b"'
alias tmp='twilio api:core:messages:create --from "+14158401410" --to "+17372327333" --body "p 6505473175 0.1"'
alias tmpold='twilio api:core:messages:create --from "+16505473175" --to "+17372327333" --body "p 4158401410 0.1"'
alias tmps='twilio api:core:messages:create --from "+14158401410" --to "+17372327333" --body "p 4158401410 0.1"'
alias tmpbp='twilio api:core:messages:create --from "+14158401410" --to "+17372327333" --body "p 4158401999 0.1"'

# 1ns aliases
alias cd1="cd ~/1ns; pwd"
alias cd1c="cd ~/1ns/ens-deployer/contract;nvm use v18.14.2; pwd"
alias cd1e="cd ~/1ns/ens-deployer/env; pwd"
alias cd1dc="cd ~/1ns/dot-country/contracts;nvm use v18.14.2; pwd"
alias cd1dce="cd ~/1ns/dot-country/env; pwd"
alias cd1g="cd ~/1ns/go-1ns; pwd"
alias cd1o="cd ~/1ns/one-wallet/opendev/johnwhitton; pwd"
alias cd1p="cd ~/1ns/coredns-1ns; pwd"
alias cd1m="cd ~/1ns/eas; pwd"
alias cd1mc="cd ~/1ns/eas/contract; nvm use v18.14.2; pwd"
alias cd1me="cd ~/1ns/eas/env; pwd"
alias cd1mf="cd ~/1ns/eas/client; nvm use v18.14.2; pwd"
alias cd1ms="cd ~/1ns/eas/server; nvm use v18.14.2; pwd"

# ens aliases (old)
alias cdhd="cd ~/1ns/hns-implementation; pwd"
alias cdhcd="cd ~/1ns/coredns; pwd"
alias cdhc="cd /Usersjohn/ens/hns-contracts; pwd"
alias cded="cd ~/1ns/docs; pwd"
alias cdea="cd ~/1ns/ens-avatar; pwd"
alias cdeaw="cd ~/1ns/ens-avatar-worker; pwd"
alias cdeec="cd ~/1ns/ens-contracts;nvm use v18.14.2; pwd"
alias cdem="cd ~/1ns/ens-metadata-service; pwd"
alias cdeg="cd ~/1ns/ens-subgraph;nvm use v14.17.0; pwd"
alias cdegn="cd ~/1ns/graph-node; pwd"
alias cdej="cd ~/1ns/ensjs-v3; pwd"
alias cdeo="cd ~/1ns/offchain-resolver; pwd"
alias cdes="cd ~/1ns/subdomain-register; pwd"
alias cdef="cd ~/1ns/ens-app; pwd"
alias cdef3="cd ~/1ns/ens-app-v3; pwd"
alias cded="cd ~/1ns/ens-deployer; pwd"
alias cde1="cd ~/1ns/dot-country; pwd"
alias cde1c="cd ~/1ns/dot-country/contracts;nvm use v18.14.2; pwd"
alias cder="cd ~/1ns/ens-registrar-relay; pwd"

# ens graph node settings
export GRAPH_ALLOW_NON_DETERMINISTIC_IPFS=1
export GRAPH_NO_EIP_1898_SUPPORT=1
export GRAPH_ETH_CALL_BY_NUMBER=1

# one-wallet aliases
alias cdo="cd ~/one-wallet; pwd"
alias cdow="cd ~/one-wallet/one-wallet; pwd"
alias cdos="cd ~/one-wallet/sms-wallet; pwd"
alias cdowww="cd ~/one-wallet/one-wallet.wiki; pwd"
alias cdowi="cd ~/one-wallet/1wallet-integration-demo; pwd"
alias cdoww="cd ~/one-wallet/wiki; pwd"
alias cdod="cd ~/one-wallet/docs; pwd"
alias cdog="cd ~/one-wallet/gitbook; pwd"
alias cdogh="cd ~/one-wallet/one-wallet-dev.github.io; pwd"
alias cdoh="cd /Users?john/one-wallet/hns; pwd"
alias cdoo="cd ~/one-wallet/opendev/one-wallet/opendev/johnwhitton; pwd"
alias gam="ganache --port 7545 --networkId 5777 -m 'filter group there hunt fitness junior ghost park route jar entire clown allow rifle meadow'"
alias gam2="ganache --port 7545 --networkId 5777 -b 2 -m 'filter group there hunt fitness junior ghost park route jar entire clown allow rifle meadow'"
alias tta='cd ~/one-wallet/one-wallet/code;pwd;truffle test test/tokens.js --network=ganache'
alias ttal='cd ~/one-wallet/one-wallet/code;pwd;VERBOSE=1 truffle test test/tokens.js --network=ganache'
alias ttt="cd ~/one-wallet/one-wallet/code;pwd;VERBOSE=0 truffle test --network=ganache --compile-none --grep 'TokenTracker'"
alias tttl="cd ~/one-wallet/one-wallet/code;pwd;VERBOSE=1 truffle test --network=ganache --compile-none --grep 'TokenTracker'"
alias ttw="cd ~/one-wallet/one-wallet/code;pwd;VERBOSE=0 truffle test --network=ganache --compile-none --grep 'Wallet_Validate'"
alias cdoi="cd ~/one-wallet/1wallet-integration-demo; pwd"
alias cdb="cd ~/one-wallet/horizon; pwd"

findportinfo() 
{  
  lsof -n -i4TCP:$1 
}
findandkill() 
{  
  port=$(lsof -n -i4TCP:$1 | grep LISTEN | awk '{ print $2 }') 
        kill -9 $port
}

alias killport=findandkill 
alias findport=findportinfo 
source /usr/local/opt/chruby/share/chruby/chruby.sh
source /usr/local/opt/chruby/share/chruby/auto.sh
chruby ruby-3.1.3
. "$HOME/.cargo/env"
eval "$(rbenv init - bash)"
#THIS MUST BE AT THE END OF THE FILE FOR SDKMAN TO WORK!!!
export SDKMAN_DIR="~/.sdkman"
[[ -s "~/.sdkman/bin/sdkman-init.sh" ]] && source "~/.sdkman/bin/sdkman-init.sh"
export PATH="/usr/local/opt/openssl@3/bin:$PATH"
export PATH="/usr/local/bin:/usr/local/sbin:$PATH"
```
