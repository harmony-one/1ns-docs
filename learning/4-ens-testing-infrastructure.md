# ENS Testing Infrastructure setup (60 mins)

1. install pre-requisites (10 minutes)
2. ens-deployer local testing (10 minutes)
3. core-dns local testing (10 minutes)
4. ens-app-v3 local testing (docker) (10 minutes)
5. ens-app-v3 local testing (20 minutes)

## install pre-requisites

See [Developer Tooling: Machine Setup (mac)](./2-developer-tooling.md#machine-setup-mac) and install all pre-requisites

- [ ] [Development Tools](./2-developer-tooling.md#development-tools) including docker
- [ ] [Javascript Development](./2-developer-tooling.md#javascript-development)
- [ ] [Solidity Development](./2-developer-tooling.md#solidity-devlopment)
- [ ] [Golang Development](./2-developer-tooling.md#golang-development)
- [ ] Create a tmp directory for this session

    ```bash
    # Create a tmp directory we will use for this session
    mkdir tmp
    cd tmp
    ```

## ens-deployer local testing

1. Create a tmp directory for lesson
2. Clone [ens-deployer](https://github.com/harmony-one/ens-deployer.git)
3. Checkout service-testing branch
4. Yarn install
5. Compile contracts
6. Test using hardhat
7. Run ganache locally
8. Deploy Contracts locally
9. Test using local network

```bash
# Open an iterm tab and split into two windows (ganache and ens-deployer

# In first window

# Clone the ens-deployer harmony-one repository
git clone https://github.com/harmony-one/ens-deployer.git
cd ens-deployer

# checkout service testing branch
git checkout service-testing

# Set node version
nvm use v18.14.2

# Initialize contracts
cd contract
yarn install
cp .env.sample .env

# Compile contracts
yarn compile

# Deploy locally
npx hardhat deploy

# Test using hardhat
npx hardhat test --grep 'Deployment'

# In second window

# Change to local environment folder
cd env

# Start local ganache instance
./ganache-new.sh

# In first window

# Test using local network
# Test deployment of contracts
yarn test-deploy
# Test creation and update of DNS records
yarn test-dns
# Test dot-country is deployed (TODO add robust testing for DC)
yarn test-dc

```

*Note: `ganache-new.sh` deletes the database every time it runs, which provides a fresh state. A hard-coded mnemonic is provided, which is also compatible with MetaMask. If you want to keep the state of ganache use `./ganache-restart`.*

## dot-country local testing

1. Clone [dot-country](https://github.com/harmony-one/dot-country)
2. Compile contracts
3. Test using hardhat
4. Run ganache locally
5. Deploy Contracts locally
6. Test using local network

```bash

# Open a 3rd window in iterm
# Clone the dot-country harmony-one repository
git clone https://github.com/harmony-one/dot-country.git
cd dot-country/

# checkout testing branch
git checkout tests

# Set node version
nvm use v18.14.2

# Initialize contracts
cd contracts
yarn install
cp .env.example .env

# Compile contracts
yarn compile

# deploy to hardhat
npx hardhat deploy

# Test using hardhat
npx hardhat test 

# Test using local network with ens contracts
# In second window
# Change to local environment folder
cd env

# Start local ganache instance
./ganache-new.sh

# In third window (dot-country contracts folder)
# Deploy 
yarn deploy-dc

# yarn test
yarn test


```

## core-dns local testing

1. Clone [coredns-1ns](https://github.com/harmony-one/coredns-1ns)
2. Build Locally
3. Run locally
4. Test a.country using DIG
5. Deploy sample dns records
6. Restest test.country using DIG
7. Clone [go-1ns](https://github.com/harmony-one/go-1ns)
8. Build locally using local go-1ns repo
9. Test a.country using DIG
10. Deploy sample dns records
11. Restest test.country using DIG

```bash
# Reference https://github.com/harmony-one/coredns-1ns/blob/main/DEVELOPER.md
# Open a fourth and fifth window in iterm

# In the fourth window

# clone the harmony-one coredns plugin
git clone https://github.com/harmony-one/coredns-1ns.git
cd coredns-1ns/

# update the dependencies
go get -u ./...
go mod tidy

# Build
go build

# Build coredns standalone
./build-standalone.sh

# Run coredns
./coredns

# In the fifth window

# Run a sample dig command: Result ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 65258
dig @127.0.0.1 a in -p 53 recurse=false test.country

# Test using local network with ens contracts
# In second window
# Change to local environment folder
cd env

# Start local ganache instance
./ganache-new.sh

# In third window (ens-deployer contracts folder)
# Deploy sample dns records
yarn deploy-sample

# In the fifth window

# Run a sample dig command: Result test.country.  3600 IN A 128.0.0.1id: 65258
dig @127.0.0.1 a in -p 53 recurse=false test.country

```

Optional buiild using local go-1ns

```bash

# Open a sixth window

# Reference https://github.com/harmony-one/go-1ns/blob/main/DEVELOPER.md

# clone the go-1ns harmony-one repository
git clone https://github.com/harmony-one/go-1ns.git
cd go-1ns

# update the dependancies 
go get -u ./...
go mod tidy
go build

# In window 4 build the plugin with local go-1ns
./build-standalone-go-1ns.sh

# Run coredns
./coredns

# In window 5 test the plugin
# Run a sample dig command: Result test.country.  3600 IN A 128.0.0.1id: 65258
dig @127.0.0.1 a in -p 53 recurse=false test.country



```

## ens-app-v3 local testing (docker)

1. Clone [ens-app-v3](https://github.com/harmony-one/ens-app-v3)
2. Run local instance using docker (see [ens-app-v3 quickstart](https://github.com/harmony-one/ens-app-v3/blob/dev/docs/Developer.md#quickstart))

```bash
# Reference https://github.com/harmony-one/ens-app-v3/blob/dev/docs/Developer.md

# clone ens-app-v3
git clone https://github.com/harmony-one/ens-app-v3.git
cd ens-app-v3/

# Set node version
nvm use v18.14.2

# Start ens-test-env (clean dataset)
# pnpm ens-test-env -a start -ns -nb --extra-time 11368000 -k
node ./ens-test-env/src/index.js -a start -s -ns -nb

# Deploy Smart Contracts
cd ens-deployer/contract
npx hardhat deploy --network local

# Install Graph and rust
# Reference https://github.com/graphprotocol/graph-node#quick-start

# Deploy Subggraph
git clone https://github.com/harmony-one/ens-subgraph.git
cd ens-subgraph
yarn setup

# Test it's working
cd ens-app-v3
pnpm buildandstart:glocal

# Stop ens-test-env using <CMD>C
# pnpm ens-test-env kill (gives error see below)

# Save the Archive file
# pnpm ens-test-env save
node ./ens-test-env/src/index.js save
rm -rf data

# load the archive file into the data folder
# pnpm ens-test-env load
node ./ens-test-env/src/index.js load

# Start ens-test-env (-nr to use existing data)
# pnpm ens-test-env -a start -nr -ns -nb --extra-time 11368000
rm -rf data
node ./ens-test-env/src/index.js load
node ./ens-test-env/src/index.js -a start -nr -ns -nb --extra-time 11368000

# Test it's working
cd ens-app-v3
pnpm buildandstart:glocal

# install dependencies
pnpm install

# Start Docker (ganache, ens-subgraph, ens-contracts, ens-metadata-service)
pnpm docker

# Start the Frontend
pnpm buildandstart:glocal
```

## ens-app-v3 local testing

1. Read [ens-app-v3 Local Development environment](https://github.com/harmony-one/ens-app-v3/blob/dev/docs/Developer.md#local-development-environment)
2. Clone related repositories
3. Install any depencies (e.g. yarn install)
4. Configure local environment
5. Perpare terminal windows ( 8 windows)
6. Start local environment
7. Register a domain

```bash
# cd to your ens directory

# Start Ganache Locally (terminal window 1)
cd ens-deployer/env
./ganache-new.sh

# Optional if you prefer to deploy via ens-deployer
cd ens-deployer/contract/contracts
# yarn install
# git checkout main
# git checkout 318d43e34fd376e2c85abe2e202976c670c0675c
yarn deploy --network local
# Updating .env.local with NEXT_PUBLIC_DEPLOYMENT_ADDRESSES generated above

# Start IPFS (terminal window 2)
cd /ipfs
ipfs daemon

# Start the postgres db (terminal window 3)
# Crate a ./postgres.sh runs the following commands
# cd ~/ens/postgres
# pg_ctl -D .postgres -l logfile stop
# cd ..
# rm -rf postgres
# mkdir postgres
# cd postgres
# initdb -D .postgres
# pg_ctl -D .postgres -l logfile start
# createdb graph-node
./postgres.sh

# Start Graph Node (terminal window 4)
# git clone https://github.com/harmony-one/graph-node.git
cd graph-node
cargo run -p graph-node --release -- /
  --postgres-url postgresql://<<username>>:<<password>>@localhost:5432/graph-node /
  --ethereum-rpc mainnet:http://127.0.0.1:8545  /
 --ipfs 127.0.0.1:5001

# Load ens-subgraph (terminal window 5)
# git clone https://github.com/harmony-one/ens-subgraph
cd ens-subgraph
nvm use v14.17.0
# yarn install
yarn setup

# Local cloudflare worker for avatar service
# Login in to cloudflare https://cloudflare.com
# git clone https://github.com/harmony-one/ens-avatar-worker
cd ens-avatar-worker
# pnpm install
pnpm start

# Local ens-metadata-service
# git clone https://github.com/harmony-one/ens-metadata-service
cd ~/ens/ens-metadata-service
git checkout local-deploy
nvm use v18.14.2
# cp .env.example.local .env
yarn install
yarn dev

# Start the Frontend (separate terminal window 6)
# git clone https://github.com/harmony-one/ens-app-v3.git
cd ~/ens/ens-app-v3
# pnpm install
# cp .env.standalone .env
pnpm dev
```
