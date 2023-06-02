# ENS Deployer

Please follow the code and steps in [ens-deployer](https://github.com/harmony-one/ens-deployer) to deploy local instances

# ENS Deployment

We are not using the standard ENS deployment process described in [official ENS documentation](https://docs.ens.domains/deploying-ens-on-a-private-chain). Instead, we use a custom ENS Deployer contract to deploy all the relevant contracts. The benefit is to have a reproducible, on-chain verifiable deployment process, and being able to have a multisig wallet to own and to trigger the deployment   deployment process. For more information, please refer to ENS Deployer repository as referenced above.