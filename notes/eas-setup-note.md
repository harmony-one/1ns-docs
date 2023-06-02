# EAS Setup Note

This tutorial details the steps required for contributors to run EAS locally. 

## Development Process

The main repo is held at [https://github.com/harmony-one/eas](https://github.com/harmony-one/eas). To get started create a [fork](https://docs.github.com/en/get-started/quickstart/fork-a-repo) of this repository. Upon completion of work create a [pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request) from your fork and optionally your [branch](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-branches) to the [main repo main branch](https://github.com/polymorpher/eas).

Throughout this document the sample fork repository is under the organization `jw-1ns`. You need to replace this with your github [account](https://docs.github.com/en/get-started/signing-up-for-github/signing-up-for-a-new-github-account) or [orgranization](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/about-organizations).

## Prerequisites

You will need the following

* [node](https://nodejs.org/en/download/): Note you may download node directly but it is recommended you use nvm below.
* [yarn](https://classic.yarnpkg.com/lang/en/docs/install/#mac-stable): `npm install --global yarn`
* [ganache](https://www.npmjs.com/package/ganache): It is recommended you install the ganache npm package using `` you may also install the [standalone application](https://trufflesuite.com/ganache/)
* [redis](https://redis.io/docs/getting-started/installation/)

The following is recommended

* [nvm](https://github.com/nvm-sh/nvm): Node version manager

*Note at time of writing it is recommended using [node v18.14.2](https://nodejs.org/en/blog/release/v18.14.2/) due to restrictions with [hardhat](https://github.com/NomicFoundation/hardhat). You can set this with the nvm command `nvm use v18.14.2`.*

This is the error you will receive if using a node version later than v18

```
error hardhat@2.12.7: The engine "node" is incompatible with this module. Expected version "^14.0.0 || ^16.0.0 || ^18.0.0". Got "19.7.0"
```

## Quickstart

Follow these steps to clone and start EAS.

Typically ganache, contracts, server and client are all run in separate sessions (terminal windows or tabs).

**Clone your forked repository**

```
git clone https://github.com/harmony-one/eas.git
```

**Redis: start Redis (to stop use Ctrl-C)**

```
redis-server
```

**Ganache: tart your local ganache environment**

```
cd eas/env
./ganache-new.sh
```

**Contracts: test and deploy the EAS contracts locally**

initial setup for contracts

```
cd eas/contract
nvm use v18.14.2
yarn install
cp .env.example .env
```

Compile EAS contracts

```
yarn compile
```

Test EAS contracts

```
yarn test
```

Deploy EAS contracts

```
yarn deploy --network local
```

**Server: start the server locally**

initial setup

```
cd eas/server
# copy the environment file
cp .env.example .env
# install dependencies
yarn install

# generate the certificates
cd certs
./gen.sh
cd ..
```

start the server

```
yarn start
```

**Client: start the frontend locally**

initial setup

```
cd eas/client
# copy the environment file
cp .env.example .env

yarn install
```

start client locally

```
yarn debug
```

