# ENS Operations

- [Deployments](https://console.cloud.google.com/compute/instances?project=radical-domain&supportedpurview=project) ENS
  - [frontend-dev](https://console.cloud.google.com/compute/instancesDetail/zones/us-west1-c/instances/frontend-dev?project=radical-domain&supportedpurview=project) <https://github.com/jw-1ns/ens-app-v3>
    - `gcloud compute ssh --zone "us-west1-c" "frontend-dev" --project "radical-domain"`
  - [graph](https://console.cloud.google.com/compute/instancesDetail/zones/us-west1-c/instances/graph?project=radical-domain&supportedpurview=project) <https://github.com/jw-1ns/ens-subgraph>
    - `gcloud compute ssh --zone "us-west1-c" "graph" --project "radical-domain"`
  - [graph-s1](https://console.cloud.google.com/compute/instancesDetail/zones/us-west1-c/instances/graph-s1?project=radical-domain&supportedpurview=project) <https://github.com/jw-1ns/ens-subgraph>
    - `gcloud compute ssh --zone "us-west1-c" "graph-s1" --project "radical-domain"`
  - [ipfs](https://console.cloud.google.com/compute/instancesDetail/zones/us-west1-c/instances/ipfs?project=radical-domain&supportedpurview=project) <https://github.com/ipfs/kubo>
    - `gcloud compute ssh --zone "us-west1-c" "ipfs" --project "radical-domain"`
  - [metadata](https://console.cloud.google.com/compute/instancesDetail/zones/us-west1-c/instances/metadata?project=radical-domain&supportedpurview=project) <https://github.com/jw-1ns/ens-metadata-service>
    - `gcloud compute ssh --zone "us-west1-c" "metadata" --project "radical-domain"`
  - [coredns](https://console.cloud.google.com/compute/instancesDetail/zones/us-west1-c/instances/coredns?project=radical-domain&supportedpurview=project) <https://github.com/coredns/coredns>
    - `gcloud compute ssh --zone "us-west1-c" "coredns" --project "radical-domain"`
  - avatar-worker <https://github.com/jw-1ns/ens-avatar-worker> (aaron deployed on cloudflare)
    - [sample cloudflare storage](https://dash.cloudflare.com/17267e599b20965cef71bfb51307928c/r2/overview)
  - All Deployments and ips

        ![HarmonyNameServiceDeployments.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cd28289c-dd1e-4466-bd34-0404d2dea658/HarmonyNameServiceDeployments.png)

- Deployments .1.country

    **Mainnet Deploy :**

    [https://ens.demo.harmony.one/](https://ens.demo.harmony.one/) (also [https://sigmoid.one/](https://sigmoid.one/) )

    [https://1.country/](https://1.country/) (also [https://isolab.country/](https://isolab.country/) )

    **Mainnet Address**: [Explorer](https://explorer.harmony.one/address/0x0ad7c7766aa67b46e4ba6fd2905531fbe62c8fc5) 0x0Ad7c7766aA67B46e4ba6Fd2905531fBE62c8Fc5

- [Google Cloud (john@modulo.so)](https://console.cloud.google.com/welcome?project=radical-domain)
  - [Setting up the cli](https://cloud.google.com/sdk/gcloud)

        ```bash
        # check gcloud is installed
        gcloud version
        
        # check credentials
        gcloud auth list
        
        # add john@modulo.so
        gclou auth login
        
        # Optionally change accounts
        gcloud auth list
        gcloud config set account john@modulo.so
        ```

  - CoreDNS Deploy using <https://github.com/jw-1ns/coredns-1ns> plugin
    - Merge web2 branch to main for ‣
    - Create a tag for ‣ and update [coredns-1ns build.sh](https://github.com/jw-1ns/coredns-1ns/blob/main/build.sh)
    - web2 → deploy branch for ‣
    - Remove local dependencies in ‣
      - `replace [github.com/jw-1ns/go-1ns](http://github.com/jw-1ns/go-1ns) => ../go-1ns` in go.mod
      - `replace [github.com/jw-1ns/coredns-1ns](http://github.com/jw-1ns/coredns-1ns) => ../../coredns-1ns` in build.sh
    - Deploy ‣ from ‣ (using main or deploy branch)
    - Sample Corefiles for ‣ and ‣ plugin
      - Corefile

                ```
                . {
                    whoami  # coredns.io/plugins/whoami
                    log     # coredns.io/plugins/log
                    errors  # coredns.io/plugins/errors
                    rewrite stop {
                    # This rewrites any requests for *.eth.link domains to *.eth internally
                    # prior to being processed by the main ONENS resolver.
                    name regex (.*)\.country\.link {1}.country
                    answer name (.*)\.country {1}.country.link
                    }
                    onens {
                        # connection is the connection to an Ethereum node.  It is *highly*
                        # recommended that a local node is used, as remote connections can
                        # cause DNS requests to time out.
                        # This can be either a path to an IPC socket or a URL to a JSON-RPC
                        # endpoint.
                        # connection /home/ethereum/.ethereum/geth.ipc
                        connection http://127.0.0.1:8545
                
                        # ethlinknameservers are the names of the nameservers that serve
                        # EthLink domains.  This will usually be the name of this server,
                        # plus potentially one or more others.
                        ethlinknameservers ns1.ethdns.xyz ns2.ethdns.xyz
                
                    }
                    forward . 8.8.8.8 9.9.9.9 # Forward all requests to Google Public DNS (8.8.8.8) and Quad9 DNS (9.9.9.9).
                }
                ```

      - Corefile.local

                ```
                . {
                    whoami  # coredns.io/plugins/whoami
                    log     # coredns.io/plugins/log
                    errors  # coredns.io/plugins/errors
                    rewrite stop {
                    # This rewrites any requests for *.eth.link domains to *.eth internally
                    # prior to being processed by the main ONENS resolver.
                    name regex (.*)\.country\.link {1}.country
                    answer name (.*)\.country {1}.country.link
                    }
                    onens {
                        # connection is the connection to an Ethereum node.  It is *highly*
                        # recommended that a local node is used, as remote connections can
                        # cause DNS requests to time out.
                        # This can be either a path to an IPC socket or a URL to a JSON-RPC
                        # endpoint.
                        # connection /home/ethereum/.ethereum/geth.ipc
                        connection http://127.0.0.1:8545
                
                        # ethlinknameservers are the names of the nameservers that serve
                        # EthLink domains.  This will usually be the name of this server,
                        # plus potentially one or more others.
                        ethlinknameservers ns1.ethdns.xyz ns2.ethdns.xyz
                
                        # ipfsgatewaya is the address of an ONENS-enabled IPFS gateway.
                        # This value is returned when a request for an A record of an Ethlink
                        # domain is received and the domain has a contenthash record in ONENS but
                        # no A record.  Multiple values can be supplied, separated by a space,
                        # in which case all records will be returned.
                        ipfsgatewaya 176.9.154.81
                
                        # ipfsgatewayaaaa is the address of an ONENS-enabled IPFS gateway.
                        # This value is returned when a request for an AAAA record of an Ethlink
                        # domain is received and the domain has a contenthash record in ONENS but
                        # no A record.  Multiple values can be supplied, separated by a space,
                        # in which case all records will be returned.
                        ipfsgatewayaaaa 2a01:4f8:160:4069::2
                    }
                    forward . 8.8.8.8 9.9.9.9 # Forward all requests to Google Public DNS (8.8.8.8) and Quad9 DNS (9.9.9.9).
                }
                ```

    - Building GCP Machine

            ```
            # Create vm and then create a firewall rule for port 53
            # vm type e2-standard-2
            # https://console.cloud.google.com/compute/instances?project=radical-domain&supportedpurview=project
            # https://stackoverflow.com/questions/21065922/how-to-open-a-specific-port-such-as-9090-in-google-compute-engine
            
            # Log into vm
            #. https://console.cloud.google.com/compute/instancesDetail/zones/us-west1-c/instances/coredns?project=radical-domain&supportedpurview=project&pageState=(%22duration%22:(%22groupValue%22:%22PT1H%22,%22customValue%22:null))
            gcloud compute ssh --zone "us-west1-c" "coredns-1ns"  --project "radical-domain"
            
            # update debian
            sudo apt update
            sudo apt upgrade
            
            # install dns utilities
            sudo apt install dnsutils
            
            # install git
            # https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
            sudo apt install git
            sudo apt install gh
            git --version
            
            # setup github on machine
            # https://docs.github.com/en/get-started/quickstart/set-up-git
            git config --global user.name "johnwhiton"
            git config --global user.name
            git config --global user.email "john@johnwhitton.com"
            git config --global user.email
            
            #create new accesss token
            # https://github.com/settings/tokens/new
            # git authentication
            
            gh auth login
            
            # install golang
            # https://go.dev/doc/install
            # Download and transfer tar file from local machine
            gcloud compute scp ~/Downloads/go1.20.linux-amd64.tar.gz coredns:/home/john
            
            # from coredns machine
            sudo rm -rf /usr/local/go 
            sudo tar -C /usr/local -xzf go1.19.4.linux-amd64.tar.gz
            export PATH=$PATH:/usr/local/go/bin
            go version
            
            # get make command
            sudo apt install make
            
            # create a repos folder
            mkdir repos
            cd repos
            ```

    - Configuration

            ```
            ∞ John ∞, [Feb 8, 2023 at 11:05:02 AM]:
            Some deployment questions
            1. Shard 0 or Shard 1 for mainnet ENS?
            
            MAINNET_URL="https://api.s0.t.hmny.io/"
            S1_URL="https://api.s1.t.hmny.io/"
            
            2. Do you have the mainnet addresses for REGISTRY AND PUBLIC_RESOLVER
            3. This name server will it be named ns3.1.country or something else?
            🙏💙👍
            
            Aaron Li, [Feb 8, 2023 at 11:06:26 AM]:
            Shard 0
            
            "registrarController": "0x32aE45799FE380c5CD76144720E11af990477C3c",
              "baseRegistrar": "0xc2370449cBf24f76a66F45D52c8134c7823F59c5",
              "resolver": "0xc39A9e4378c0BDF89f34788e31B4FBD5744B8709",
            
            - ens deployed to: 0x54E39ED8d57250cf1378a8435696efe826Fb4504
            
            ns3.hiddenstate.xyz
            ```

    - Build ‣ on GCP

            ```bash
            # Log into vm
            #. https://console.cloud.google.com/compute/instancesDetail/zones/us-west1-c/instances/coredns?project=radical-domain&supportedpurview=project&pageState=(%22duration%22:(%22groupValue%22:%22PT1H%22,%22customValue%22:null))
            gcloud compute ssh --zone "us-west1-c" "coredns-1ns"  --project "radical-domain"
            
            # Clone the repo
            git clone https://github.com/jw-1ns/coredns-1ns.git
            cd coredns-1ns
            
            # Build coredns with coredns-1ns plugin
            ./build.sh
            
            # Ensure you have a .env file
            
            # set up tmux and run in a tmux session
            # https://tmuxcheatsheet.com/
            sudo apt install tmux
            tmux new -s coredns
            sudo ./coredns 
            # to exit use
            <ctrl>b d 
            
            # to reattach to an existing session
            tmux attach -t coredns
            
            ```

  - CoreDNS Deploy
    - VM Deploy [coredns vm](https://console.cloud.google.com/compute/instancesDetail/zones/us-west1-c/instances/coredns?project=radical-domain&supportedpurview=project)
      - ‣ [go install](https://go.dev/doc/install)
      - Installing from source code

                ```bash
                # Create vm and then create a firewall rule for port 53
                # vm type e2-standard-2
                # https://console.cloud.google.com/compute/instances?project=radical-domain&supportedpurview=project
                # https://stackoverflow.com/questions/21065922/how-to-open-a-specific-port-such-as-9090-in-google-compute-engine
                
                # Log into vm
                #. https://console.cloud.google.com/compute/instancesDetail/zones/us-west1-c/instances/coredns?project=radical-domain&supportedpurview=project&pageState=(%22duration%22:(%22groupValue%22:%22PT1H%22,%22customValue%22:null))
                gcloud compute ssh --zone "us-west1-c" "coredns"  --project "radical-domain"
                
                # update debian
                sudo apt update
                sudo apt upgrade
                
                # install dns utilities
                sudo apt install dnsutils
                
                # install git
                # https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
                sudo apt install git
                sudo apt install gh
                git --version
                
                # setup github on machine
                # https://docs.github.com/en/get-started/quickstart/set-up-git
                git config --global user.name "johnwhiton"
                git config --global user.name
                git config --global user.email "john@johnwhitton.com"
                git config --global user.email
                
                #create new accesss token
                # https://github.com/settings/tokens/new
                # git authentication
                
                gh auth login
                
                # install golang
                # https://go.dev/doc/install
                # Download and transfer tar file from local machine
                gcloud compute scp ~/Downloads/go1.20.linux-amd64.tar.gz coredns:/home/john
                
                # from coredns machine
                sudo rm -rf /usr/local/go 
                sudo tar -C /usr/local -xzf go1.19.4.linux-amd64.tar.gz
                export PATH=$PATH:/usr/local/go/bin
                go version
                
                # get make command
                sudo apt install make
                
                # create a repos folder
                mkdir repos
                cd repos
                
                # Clone coredns deploy
                git clone https://github.com/coredns/deployment.git
                
                # Clone and build coredns
                git clone https://github.com/coredns/coredns
                cd coredns
                # add in the records plugin
                # https://coredns.io/explugins/records/
                # vim plugin.cfg and add `records:github.com/coredns/records` after `hosts:hosts`
                make
                
                # Create a coredns folder and copy files over
                cd ~
                mkdir coredns
                cp ../repos/coredns/coredns .
                cp ../repos/deployment/debian/Corefile .
                
                # Update the configuration file
                # default A record to 34.120.199.241 for all domains
                # https://en.wikipedia.org/wiki/Wildcard_DNS_record
                # https://help.dyn.com/how-to-format-a-zone-file/
                # https://www.rfc-editor.org/rfc/inline-errata/rfc1035.html
                # https://coredns.io/plugins/file/
                
                ## Corefile
                ```

# Default Corefile, see <https://coredns.io> for more information

# Answer every below the root, with the whoami plugin. Log all queries

# and errors on standard output

                . {
                    whoami  # coredns.io/plugins/whoami
                    log     # coredns.io/plugins/log
                    errors  # coredns.io/plugins/errors
                    file db.country country # Hardcode default IP for .country
                    forward . 8.8.8.8 9.9.9.9 # Forward all requests to Google Public DNS (8.8.8.8) and Quad9 DNS (9.9.9.9).
                }

                ```
                
                ## db.country file
                
                ```

                $ORIGIN country.
                @  3600 SOA 127.0.0.1:53 help.country. (
                    0000000001 ; serial
                    7200       ; refresh (2 hours)
                    3600       ; retry (1 hour)
                    1209600    ; expire (2 weeks)
                    3600       ; minimum (1 hour)
                    )

                *.country. 3600 IN A     34.120.199.241
                  3600 IN A     34.120.199.241
                *.country.      3600 IN AAAA  34.120.199.241
                         3600 IN AAAA  34.120.199.241
                www  3600 IN CNAME .country.

                ```
                
                # set up tmux and run in a tmux session
                # https://tmuxcheatsheet.com/
                sudo apt install tmux
                tmux new -s coredns
                sudo ./coredns 
                # to exit use
                <ctrl>b d 
                
                # to reattach to an existing session
                tmux attach -t coredns
                
                # Test coredns from laptop
                ## Test you can access the port
                nc -zv 34.168.139.126 53
                
                ## Test the dns resolution
                ## https://hackmd.io/@linnil1/ByAUHOezs
                dig -p 53 @34.168.139.126 test.country
                nslookup -port=53 fred.country 34.168.139.126
                nslookup -port=53 google.com 34.168.139.126
                ```

      - Other deployment options
        - Debian Install

                    ```bash
                    # Log into vm
                    #. https://console.cloud.google.com/compute/instancesDetail/zones/us-west1-c/instances/coredns?project=radical-domain&supportedpurview=project&pageState=(%22duration%22:(%22groupValue%22:%22PT1H%22,%22customValue%22:null))
                    gcloud compute ssh --zone "us-west1-c" "coredns"  --project "radical-domain"
                    
                    # Debian Initialization
                    # https://cloud.google.com/artifact-registry/docs/os-packages/debian/configure
                    sudo apt update
                    sudo apt install apt-transport-artifact-registry
                    
                    # Debian setup
                    # https://github.com/coredns/deployment#debian
                    dpkg-buildpackage -us -uc -b
                    
                    # Installation
                    
                    ```

        - Kubernetes Deploy
          - ‣ [GKE Tutorial](https://cloud.google.com/kubernetes-engine/docs/deploy-app-cluster?hl=en_US) [Deploying Containers on VM’s](https://cloud.google.com/compute/docs/containers/deploying-containers)
      - Changing the default ip

                ```bash
                # log in to the coredns gcloud instance
                gcloud compute ssh --zone "us-west1-c" "coredns"  --project "radical-domain"
                # to reattach to an existing session
                tmux attach -t coredns
                
                #kill the running sessions
                <CTRL> C
                
                # edit the db.country file
                vim /home/john/coredns/db.country
                
                # Change the IP addresses as follow
                ```

                $ORIGIN country.
                @               3600    SOA 127.0.0.1:53 help.country. (
                                                0000000001 ; serial
                                                7200       ; refresh (2 hours)
                                                3600       ; retry (1 hour)
                                                1209600    ; expire (2 weeks)
                                                3600       ; minimum (1 hour)
                                                )

                *.country.      3600 IN A     34.107.176.102
                                3600 IN A     34.107.176.102
                *.country.      3600 IN AAAA  34.107.176.102
                                3600 IN AAAA  34.107.176.102
                www             3600 IN CNAME .country.

                ```
                
                # restart coredns
                sudo ./coredns 
                # to exit use
                <ctrl>b d 
                
                # To check the update is working go to 
                # https://isolab.country/
                # you should see the .country on the web page
                # ISOLAB .country
                ```

    - Reference Docs
      - [https://coredns.io/](https://coredns.io/)
      - [https://medium.com/google-cloud/coredns-afaa732aa35e](https://medium.com/google-cloud/coredns-afaa732aa35e)
      - [https://medium.com/google-cloud/using-coredns-on-gke-3973598ab561](https://medium.com/google-cloud/using-coredns-on-gke-3973598ab561)
      - [https://coredns.io/plugins/clouddns/](https://coredns.io/plugins/clouddns/)
- [Local ENS Environment](https://github.com/jw-1ns/ens-app-v3/blob/multichain-docker-simplified/docs/Developer.md)
  - Updating Docker Build - Deploying Contracts and Saving Archive

        ```bash
        # Start ens-test-env (clean dataset)
        # pnpm ens-test-env -a start -ns -nb --extra-time 11368000 -k
        node ./ens-test-env/src/index.js -a start -s -ns -nb
        
        # Deploy Smart Contracts
        cd ens-deployer/contract
        npx hardhat deploy --network local
        
        # Deploy Subggraph
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
        ```

  - Updating Configuration Files

## Local Development Configuration

        The main configuration settings are

        1. Configuring the frontend to use Ganache and Subgraph, this is done in your [.env](https://www.notion.so/.env) file, you can see [.env.example](https://www.notion.so/.env.example) for sample configuration

        ```
        SECRET_WORDS="test test test test test test test test test test test junk"
        PASSWORD=TestMetaMask
        NETWORK_NAME=localhost 
        RPC_URL=http://127.0.0.1:8545
        GRAPH_URL=http://localhost:8000
        
        DATA_FOLDER=./data
        
        NEXT_PUBLIC_ALCHEMY_KEY=sSpYuHmhlpuU7RVXq-IIdCdz4IuKF-gM
        
        CHAIN_ID=1337
        BLOCK_HEIGHT=12066620
        SUBGRAPH_ID=QmXxAE7Urtv6TPa8o8XmPwLVQNbH6r35hRKHP63udTxTNa
        LOCAL_SUBGRAPH_ID=QmSUnR4AUTQ8CuGH2fK7tFTSSfYGe8BUz6EeBRNavXbE1H
        EPOCH_TIME=1660180306
        NETWORK=local
        ARCHIVE_URL=https://storage.googleapis.com/ens-manager-build-data
        
        TRANSACTION_WAIT_TIME=5000
        STABLE_MODE=500
        BATCH_GATEWAY_URLS=["test.eth"]
        NEXT_PUBLIC_PROVIDER=http://127.0.0.1:8545
        NEXT_PUBLIC_GRAPH_URI=http://localhost:8000/subgraphs/name/graphprotocol/ens
        NEXT_METADATA_SERVICE=http://localhost:8081
        NEXT_PUBLIC_AVUP_ENDPOINT=http://localhost:8787/local
        
        ```

        1. Configuring your deployed contract is done in [.env.local](https://www.notion.so/.env.local). This is done automatically when running `npx hardhat deploy --network localhost` or you can set manually if you are using the [ens-deployer](https://github.com/harmony-domains/ens-deployer). It is set as follows

        ```
        NEXT_PUBLIC_DEPLOYMENT_ADDRESSES='{"LegacyENSRegistry":"0x5FbDB2315678afecb367f032d93F642f64180aa3","ENSRegistry":"0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0","RSASHA1Algorithm":"0xCf7Ed3AccA5a467e9e704C703E8D87F634fB0Fc9","RSASHA256Algorithm":"0xDc64a140Aa3E981100a9becA4E685f962f0cF6C9","P256SHA256Algorithm":"0x5FC8d32690cc91D4c39d9d3abcBD16989F875707","DummyAlgorithm":"0x0165878A594ca255338adfa4d48449f69242Eb8F","SHA1Digest":"0xa513E6E4b8f2a923D98304ec87F64353C4D5C853","SHA256Digest":"0x2279B7A0a67DB372996a5FaB50D91eAA73d2eBe6","DummyDigest":"0x8A791620dd6260079BF849Dc5567aDC3F2FdC318","DNSSECImpl":"0x610178dA211FEF7D417bC0e6FeD39F05609AD788","TLDPublicSuffixList":"0xc6e7DF5E7b4f2A278906862b61205850344D4e7d","DNSRegistrar":"0x59b670e9fA9D0A427751Af201D676719a970857b","Root":"0x4ed7c70F96B99c776995fB64377f0d4aB3B0e1C1","BaseRegistrarImplementation":"0x4A679253410272dd5232B3Ff7cF5dbB88f295319","DummyOracle":"0x09635F643e140090A9A8Dcd712eD6285858ceBef","ExponentialPremiumPriceOracle":"0xc5a5C42992dECbae36851359345FE25997F5C42d","StaticMetadataService":"0x67d269191c92Caf3cD7723F116c85e6E9bf55933","NameWrapper":"0xE6E340D132b5f46d1e472DebcD681B2aBc16e57E","LegacyPublicResolver":"0x84eA74d481Ee0A5332c457a4d796187F6Ba67fEB","ReverseRegistrar":"0x9E545E3C0baAB3E08CdfD552C960A1050f373042","LegacyETHRegistrarController":"0x1613beB3B2C4f22Ee086B2b38C1476A3cE7f78E8","ETHRegistrarController":"0x851356ae760d987E095750cCeb3bC6014560891C","PublicResolver":"0x95401dc811bb5740090279Ba06cfA8fcF6113778","UniversalResolver":"0x998abeb3E57409262aE5b751f60747921B33613E","BulkRenewal":"0x70e0bA845a1A0F2DA3359C97E0285013525FFC49","Multicall":"0x4826533B4897376654Bb4d4AD88B7faFD0C98528","resolverMulticallWrapper":"0x95401dc811bb5740090279Ba06cfA8fcF6113778"}'
        
        ```

  - Running a Local ENS Environment (or [docker commands in ens-app-v3)](https://github.com/jw-1ns/ens-app-v3/blob/dev/docs/Developer.md)

        ```bash
        # Start Ganache Locally (terminal window 1)
        cd ~/ens/ens-deployer/env
        ./ganache-new.sh
        
        # Optional if you prefer to deploy via ens-deployer
        cd ~/ens/ens-deployer/contract/contracts
        yarn deploy --network local
        # Updating .env.local with NEXT_PUBLIC_DEPLOYMENT_ADDRESSES generated above
        
        # Start IPFS (terminal window 2)
        cd ~/ens/ipfs
        ipfs daemon
        
        # Start the postgres db (terminal window 3)
        # ./postgres.sh runs the following commands
        # cd ~/ens/postgres
        # pg_ctl -D .postgres -l logfile stop
        # cd ..
        # rm -rf postgres
        # mkdir postgres
        # cd postgres
        # initdb -D .postgres
        # pg_ctl -D .postgres -l logfile start
        # createdb graph-node
        cd ~/ens
        ./postgres.sh
        
        # Start Graph Node (terminal window 4)
        cd ~/ens/graph-node
        cargo run -p graph-node --release -- /
          --postgres-url postgresql://<<username>>:<<password>>@localhost:5432/graph-node /
          --ethereum-rpc mainnet:http://127.0.0.1:8545  /
         --ipfs 127.0.0.1:5001
        
        # Load ens-subgraph (terminal window 5)
        cd ~/ens/ens-subgraph
        nvm use v14.17.0
        yarn setup
        
        # Local cloudflare worker for avatar service
        # Login in to cloudflare https://cloudflare.com
        cd ~/ens/ens-avatar-worker
        pnpm start
        
        # Local ens-metadata-service
        cd ~/ens/ens-metadata-service
        yarn dev
        
        # Start the Frontend (separate terminal window 6)
        cd ~/ens/ens-app-v3
        nvm use v14.17.0
        pnpm dev
        ```

- Deploying Relayer <https://github.com/jw-1ns/ens-registrar-relay>
