---
title: Validator Setup version 2
---

## Version 2 updates

Find the full list of exact updates **20/09/2021**
1. Add new section [How to separate your running nodes](/build/validator.html#how-to-separate-your-running-nodes)
2. Update the parameter **PRIVATE_PEERS** in the step 6 within the section [Configure and start the Sentry node as a validator](/build/validator.html#configure-and-start-the-sentry-node-as-a-validator)
3. Update the parameter **PRIVATE_PEERS** in the step 6 within the section [Configure and start the Seed node as a validator](/build/validator.html#configure-and-start-the-seed-node-as-a-validator)
4. Update the [Ethereum full-node](/build/validator.html#ethereum-full-node) section with the [standard recommendation and specification](https://ethereum.org/en/developers/docs/nodes-and-clients/#recommended-specifications)
5. Add new section [How to delete a current running node](/build/validator.html#how-to-delete-a-current-running-node)

## How to delete a current running node

If you stop the docker container that is running a Full node then you are not able to use it. But if you want to remove the full node docker data then you need to clear the volume of full node docker, if you remove the folder it will remove all the data but make sure first that you stop the docker container.

Clear the volume of full node docker:
* Navigate and open the file **CudosBuilders/docker/full-node/full-node.client.testnet.public01.arg**
* Find the var **VOLUME_NAME=cudos-data-full-node-client-testnet-public-01** and clear it
* Navigate to the file **CudosBuilders/docker/full-node/start-full-node.yml**
* Find the one volume field
volumes: **- '../../../CudosData/$VOLUME_NAME:$CUDOS_HOME'**
* Above **VOLUME_NAME**  is mapped with this **../../../CudosData/$VOLUME_NAME**, clear it

Remove the folder:
Navigate to the folder **CudosData**, you may find a folder known as **cudos-data-full-node-client-testnet-public-01**, this is the folder which store all data of full node and needs to be removed.

## How to run a Cudos Validator Node

As explained in the article [Types of Nodes](/learn/validators.html#types-of-nodes) there are three types of nodes: Full, Sentry, and Seed node.

For one or more validator nodes it is recommended to launch a layer of sentry nodes (at least 1 Sentry node) and optionally Seed nodes with isolating the validator node behind that layer.

You need an IP-address per node which is directly connected to the network. For example, If you have **N** validator nodes and only one Sentry node then only the Sentry node is directly connected to the network. In this case you will need a single IP-address.

## Validator Setup

### How to separate your running nodes

As a validator, in order to have a secure and running network, you will need to run each of the following nodes on a different machine:

1. Validator Full node along with the Orchestrator configuration on a separate local machine
2. Sentry node on a separate local machine
3. Seed node on a separate local machine
4. Ethereum full node, you can run it virtually on a cloud, if you do not have an Ethereum full node then you need to run the Orchestrator configuration on a separate local machine (not along with validator node).

The picture below shows the diagram of validator topology:

![img](./validator-topoligy.jpg)

This article guides you through the instruction for running each one of those nodes.

### ﻿Run a Full node

Before running a node, make sure that you have followed the guide for [setting up your prerequisites and environment](/build/prerequisites.html).

To run a full node, you need to complete the following steps:

- Initialize the node
- Configure and start the node

#### Initialize the Full node as a validator

When you run a validator node, you play an important role in the security of a network. A validator must be secure and fault-tolerant. So it is recommended to run your validator with a layer of 1 or more sentry nodes and to isolate the validator node behind that layer. Also, you will need an IP-address per node that is connected to the network. For example, if you have 10 validator nodes and only one Sentry node then only the Sentry node will be connected to the network where you will need a single IP-address.

In total, Cudos has [three types of nodes](/learn/validators.html#types-of-nodes) and as a validator, you need to have at least:
* one Sentry node
* one Full node

You can initialize the full node as the following:

1. Navigate to the directory *CudosBuilders/docker/full-node*
2. Find the file **full-node.env.example** and create a copy of it
3. Rename the copied file to **full-node.client.testnet.public01.env**
4. Open the file, which you renamed, **full-node.client.testnet.public01.env** in any editor
5. Find both the **"MONIKER"** attribute and the flag **"SHOULD_USE_GLOBAL_PEERS"** and set them as the following :
```
MONIKER=MyFullNodeName
SHOULD_USE_GLOBAL_PEERS=false
```
6. Leave other variables such as "PERSISTENT_PEERS" and "SEEDS" empty and save the changes that you made to the file.
7. Make sure that you are still in the correct directory **CudosBuilders/docker/full-node**
8. Initialize the node by running this command:
```
sudo docker-compose --env-file full-node.client.testnet.public01.arg -f init-full-node.yml -p cudos-init-full-node-client-testnet-public-01 up --build
```

If all steps are completed successfully, you should see a newly generated folder called **CudosData** at the same directory where you placed *CudosBuilders* and *CudosNode*. The subdirectory *cudos-data-full-node-client-testnet-public-01* of **CudosData"** folder has a file called **tendermint.nodeid**. This file contains your node **Id,** to see your node id you can open this file in any code editor and you will get one line that represents your node id such as 13f359c90582b12e291311980a855854668d80pc.

#### Configure and start the Full node

This step is valid only if you are running the full node as a validator. Note that if you are not a validator, you do not need to follow this step.

Now you need to configure and start the full node. So far the full node is set to be isolated and to connect the full node to the network, it needs Sentry peers. The full node should run behind the layer of running a Seed node and a Sentry node with all necessary configuration and starting the node as a validator.

There are two different parameters for selecting the way to connect peers:
* **PERSISTENT_PEERS** are list of peers that your current node is ALWAYS connected to. It contains a list of comma separated peers that you will always want to be connected to.
* **PRIVATE_PEERS** are list of peers that your current node does not share and it is totally private. For example - the Sentry/Seed node MUST set its validator (if available) as a private peer in order not to avoid sharing your validator's id/ip to the rest of the network. So it is a comma-separated list of node ids that will not be exposed to other peers which can be filled with a validator’s node id.

The full node must communicate only through the created layer of peers. To achieve that, you will need to apply the following steps:

1. [Run a Sentry node](#run-a-sentry-node), configure, and start it as a validator
2. [Run a Seed node](#run-a-seed-node), configure, and start it as a validator. This is an optional recommended step.
3. Get the Sentry and Seed **node ids** and add them inside the file **full-node.client.testnet.public01.env**. Leave the variable _SEEDS_ empty if you do not have seed nodes. If you have more than one Sentry or Seed node, you can separate them by a comma within the variable _PERSISTENT_PEERS_:
```
PERSISTENT_PEERS=<sentry-node1-id>@<sentry-node1-ip>:26656,<sentry-node2-id>@<sentry-node2-ip>:26656
SEEDS=<seed-node1-id>@<seed-node1-ip>:26656,<seed-node2-id>@<seed-node2-ip>:26656
```
4. Open the terminal and navigate to **CudosBuilders/docker/full-node**
5. Configure your node:
```
sudo docker-compose --env-file full-node.client.testnet.public01.arg -f config-full-node.yml -p cudos-config-full-node-client-testnet-public-01 up --build
```
6. Start your node
```
sudo docker-compose --env-file full-node.client.testnet.public01.arg -f start-full-node.yml -p cudos-start-full-node-client-testnet-public-01 up --build --detach
```

Note that you can see the logs by running the command:
```
sudo docker logs -f cudos-start-full-node-client-testnet-public-01
```

#### Initialize and start the Full node without being a validator

If you are not a validator and you want to initialize a full node, all you need to do is to follow the same steps for [Initialize the node as a validator](#configure-and-start-the-node-as-a-validator) but make sure that you set the flag **"SHOULD_USE_GLOBAL_PEERS"** to true. To do that, open the file **full-node.client.testnet.public01.env.** in any editor then set the flag to true:
```
SHOULD_USE_GLOBAL_PEERS=true
```

You can start your Full node by running the command:
```
sudo docker-compose --env-file full-node.client.testnet.public01.arg -f start-full-node.yml -p cudos-start-full-node-client-testnet-public-01 up --build --detach
```

### ﻿Run a Sentry node

Before running a node, make sure that you have followed the guide for [setting up your prerequisites and environment](/build/prerequisites.html).

To run a sentry node, you need to complete the following steps:

- Initialize the node
- Configure and start the node

#### Initialize and start the Sentry node without being a validator

1. Navigate to the directory *CudosBuilders/docker/sentry-node*
2. Find the file **sentry-node.env.example** and create a copy of it
3. Rename the copied file to **sentry-node.client.testnet.public01.env**
4. Open the file, which you renamed, **sentry-node.client.testnet.public01.env** in any editor
5. Find  the **"MONIKER"** attribute and set a name to it as the following:
```
MONIKER=MySentryNodeName
```
6. Initiliaze the node by running this command:
```
sudo docker-compose --env-file sentry-node.client.testnet.public01.arg -f init-sentry-node.yml -p cudos-init-sentry-node-client-testnet-public-01 up --build
```
7. Start the node by running this command:
```
sudo docker-compose --env-file sentry-node.client.testnet.public01.arg -f start-sentry-node.yml -p cudos-start-sentry-node-client-testnet-public-01 up --build
```

#### Configure and start the Sentry node as a validator

This step is valid only if you are running the sentry node as a validator. Note that if you are not a validator, you do not need to follow this step.

1. Navigate to the directory *CudosBuilders/docker/sentry-node*
2. Find the file **sentry-node.env.example** and create a copy of it
3. Rename the copied file to **sentry-node.client.testnet.public01.env**
4. Open the file, which you renamed, **sentry-node.client.testnet.public01.env** in any editor
5. Find  the **"MONIKER"** attribute and set a name to it as the following:
```
MONIKER=MySentryNodeName
```
6. Paste the full node's nodeId in the **PRIVATE_PEERS** line. If there are multiple full nodes ids, separate them by a comma such as:
```
PRIVATE_PEERS=<full-node1-id>@<full-node1-ip>:26656,<full-node2-id>@<full-node2-ip>:26656
```
7. Make sure that you are still in the correct directory **CudosBuilders/docker/sentry-node**
8. Initialize the node by running this command:
```
sudo docker-compose --env-file sentry-node.client.testnet.public01.arg -f init-sentry-node.yml -p cudos-init-sentry-node-client-testnet-public-01 up --build
```
9. Start the node by running this command:
```
sudo docker-compose --env-file sentry-node.client.testnet.public01.arg -f start-sentry-node.yml -p cudos-start-sentry-node-client-testnet-public-01 up --build --detach
```

Note that you can see the logs by running the command:
```
sudo docker logs -f cudos-start-sentry-node-client-testnet-public-01
```

### ﻿Run a Seed node

Before running a node, make sure that you have followed the guide for [setting up your prerequisites and environment](/build/prerequisites.html).

To run a seed node, you need to complete the following steps:

- Initialize the node
- Configure and start the node

#### Initialize and start the Seed node without being a validator

1. Navigate to the directory *CudosBuilders/docker/seed-node*
2. Find the file **seed-node.env.example** and create a copy of it
3. Rename the copied file to **seed-node.client.testnet.public01.env**
4. Open the file, which you renamed, **seed-node.client.testnet.public01.env** in any editor
5. Find  the **"MONIKER"** attribute and set a name to it as the following:
```
MONIKER=MySeedNodeName
```
6. Initiliaze the node by running this command:
```
sudo docker-compose --env-file seed-node.client.testnet.public01.arg -f init-seed-node.yml -p cudos-init-seed-node-client-testnet-public-01 up --build
```
7. Start the node by running this command:
```
sudo docker-compose --env-file seed-node.client.testnet.public01.arg -f start-seed-node.yml -p cudos-start-seed-node-client-testnet-public-01 up --build
```

#### Configure and start the Seed node as a validator

This step is valid only if you are running the seed node as a validator. Note that if you are not a validator, you do not need to follow this step.

1. Navigate to the directory *CudosBuilders/docker/seed-node*
2. Find the file **seed-node.env.example** and create a copy of it
3. Rename the copied file to **seed-node.client.testnet.public01.env**
4. Open the file, which you renamed, **seed-node.client.testnet.public01.env** in any editor
5. Find  the **"MONIKER"** attribute and set a name to it as the following:
```
MONIKER=MyseedNodeName
```
6. Paste the full node's nodeId in the **PRIVATE_PEERS** line. If there are multiple full nodes ids, separate them by a comma such as:
```
PRIVATE_PEERS=<full-node1-id>@<full-node1-ip>:26656,<full-node2-id>@<full-node2-ip>:26656
```
7. Make sure that you are still in the correct directory **CudosBuilders/docker/seed-node**
8. Initialize the node by running this command:
```
sudo docker-compose --env-file seed-node.client.testnet.public01.arg -f init-seed-node.yml -p cudos-init-seed-node-client-testnet-public-01 up --build
```
9. Start the node by running this command:
```
sudo docker-compose --env-file seed-node.client.testnet.public01.arg -f start-seed-node.yml -p cudos-start-seed-node-client-testnet-public-01 up --build --detach
```

Note that you can see the logs by running the command:
```
sudo docker logs -f cudos-start-seed-node-client-testnet-public-01
```

## Create a validator

To create a validator account, you need:
1. A running Ethereum full-node
2. A running Cudos Full node ,which has the validator configuration, and a setup of the orchestrator.

Only after finalizing previous steps, you can start the process of creating a validator. this section explains how to achieve each step in detail.

### Ethereum full node

First make sure that you have the [standard recommendation and specification](https://ethereum.org/en/developers/docs/nodes-and-clients/#recommended-specifications) for the Ethereum node.

You can use either an existing [Ethereum full-node](https://ethereum.org/en/developers/docs/nodes-and-clients/#full-node) (if you have one) or you can follow the procedure below to start one but make sure not to use Infura:

1. Run your Ethereum binary on a different machine that your validator is running
2. Clone the correct branch from the [CudosBuilders](https://github.com/CudoVentures/cudos-builders) repository with renaming the folders accordingly to exactly _CudosBuilders_:
```
git clone --depth 1 --branch sdk-0.43  https://github.com/CudoVentures/cudos-builders.git CudosBuilders
```
3. Open shell, navigate to the directory _CudosBuilders/docker/ethereum_ and start the Ethereum full-node by running the command:
```
cd CudosBuilders/docker/ethereum && sudo docker-compose -f ethereum-full.yml -p ethereum up --build --detach
```

Note that you have to wait ~12 hours to finish syncing the Rinkeby test network. Its size is almost 70GB. You can see the logs by running the command:
```
sudo docker logs -f ethereum
```

### Cudos Validator node and Orchestrator

Make sure that you are [running Cudos full-node as a validator](#validator-setup)

Access the container, that is needed to connect to its bash, directly with its name by running the command:
```
sudo docker exec -it cudos-start-full-node-client-testnet-public-01 bash
```

1. As a first step, you need to get the private key of your node. So, if you created the account by Keplr then just connect to the full nodes' container and run the following commands to add it to the node:
```
# The amount you want to stake, denominate them in acudos, without spaces (min 1 000 000 000 000 000 000 acudos) export
export STAKE="1000000000000000000acudos"
export CHAIN_ID="cudos-testnet-public"

# Add the wallet in your nodes' keyring:
cudos-noded keys add validator --recover --keyring-backend="os"
```
2. Enter your mnemonic address for the account, your account should hold at least 1000000000000000000acudos (1 CUDOS).
3. Create a password, which will be used to lock the internal Keystore
4. Re-enter the password
5. You can change the rates as you desire for your validator
6. Create a validator by entering the password and running the following command (change the rates with the ones you want for your validator):
```
cudos-noded tx staking create-validator --amount=$STAKE \
    --from=validator \
    --pubkey=$(cudos-noded tendermint show-validator) \
    --moniker=$MONIKER \
    --chain-id=$CHAIN_ID \
    --commission-rate="0.10" \
    --commission-max-rate="0.20" \
    --commission-max-change-rate="0.01" \
    --min-self-delegation="1" \
    --gas="auto" \
    --gas-prices="0.025acudos" \
    --gas-adjustment="1.80" \
    --keyring-backend="os" \
    -y
```
7. If you see the transaction hash without getting any error, then congrats you have successfully created a validator account.

Note that if you get a message that the transaction is not included in any block, please wait a few seconds and do not start another transaction.

:::tip
Be aware not to exit the docker shell. You will need it for the next step that is registering the orchestrator.
:::

### Orchestrator

The orchestrator is a program that runs on every validator beside the Cudos code. Validators, running a chain with the Gravity module installed, use the orchestrator to sign messages or transactions with a validator's unique key.

#### Get the validator address

After you have created a validator account, you must find your validator address by running the command:
```
cudos-noded q staking validators
```

the resulting output looks similar to the picture below, you need to find your validator, you can refer to the moniker to find it, and copy its **operator_address**:
![](./Cosmos-orchestrator1.png)

#### Add the orchestrator wallet

Now you need to add another wallet to use for the orchestrator but first make sure that **it has some CUDOS tokens**. You can achieve that by running the command:
```
cudos-noded keys add orchestrator --recover --keyring-backend="os"
```

:::tip
Note that after running the command above, you will need to enter both your mnemonic address for the account and the password which you have created on a previous step while adding the validator's wallet.
:::

The resulting output looks similar to the picture below. You will need the address of this wallet and mnemonic for the next steps.

![](./Cosmos-orchestrator2.png)

#### Register and run the orchestrator

1. Add the following variables:
```
export VALIDATOR_ADDRESS="<*operator_address* from above>"
export ORCH_ADDRESS="<*address* from the previous step>"
export ETH_ADDRESS="<eth address, starting with 0x, that have some ETH on rinkeby test network>"
```
2. Register the orchestrator:
```
cudos-noded tx gravity set-orchestrator-address $VALIDATOR_ADDRESS $ORCH_ADDRESS $ETH_ADDRESS --from validator --keyring-backend "os" --chain-id $CHAIN_ID
```

Now let's run the orchestrator, please make sure to run your gravity bridge binary on the same machine that your validator node is running on.
1. Open shell and navigate to the directory _CudosBuilders/docker/orchestrator_
2. Create a copy of **orchestrator.env.example**
3. Rename it to **orchestrator.client.testnet.public01.env**
4. Open the file _orchestrator.client.testnet.public01.env_ in any editor and set all of the parameters.
5. Delete any comments from this file (delete # and everything after it), the parameter **GRPC** is the port value of the Sentry node.
```
ADDRESS_PREFIX="cudos" # ADDRESS_PREFIX must be exactly as here
FEES="<fee that you will have to pay for each bridge operation>" # format "100acudos"
GRPC="http://<ip of your cudos node>:9090" # port should be 9090
ETHRPC="http://<ip of ethereum node>:8545" # port should be 8545
CONTRACT_ADDR="0xb22F2A4c231e69703FC524Eb2E3eb7B83C316F42" # CONTRACT_ADDR must be exactly as here
COSMOS_ORCH_MNEMONIC="<mnemonic of your orchestrator account>"
ETH_PRIV_KEY_HEX="<private key of your eth wallet that was used to register the validator>" # in hex format without leading 0x
```
6. Finally run the orchestrator
```
sudo docker-compose --env-file orchestrator.client.testnet.public01.arg -f orchestrator.release.yml -p cudos-orchestrator-client-testnet-public-01-release up --build --detach
```

you can see the logs by running the command:
```
sudo docker logs -f cudos-orchestrator-client-testnet-public-01-release
```

### Send funds using the gravity bridge

You have two different options to send funds (it is recommended to use the first option UI):
1. Using gravity bridge UI
2. Using the console

#### Using the gravity bridge UI (recommended option)

Open [Gravity Bridge](http://35.192.177.142:4000/). Then you can use [Kelpr](https://wallet.keplr.app/) and [Metamask](https://metamask.io/) for sending funds between the two blockchains.

#### Using the console (not recommended option)

1. Start docker shell once again, like you did when you have created your validator
2. Connect to the orchestrator instance instead of the validator one
3. Choose how you want to send funds
4. Before sending funds to Ethereum please check the available balance in the smart contract on the address.
5. Send funds from Ethereum to Cudos by running the command:
```
./gbt client eth-to-cosmos \
  --ethereum-key "<private key of the sender in hex without leading 0x>" \
  --gravity-contract-address "0x9fdE6D55dDa637806DbF016a03B6970613630333" \
  --amount <amount in CUDOS without ""> \ #example 0.000000000000000001
  --destination "<destination cudos address>" \
  --token-contract-address "0x28ea52f3ee46cac5a72f72e8b3a387c0291d586d" \
  --ethereum-rpc "http://<ip of your ethereum node>:8545"
```
6. Send funds from Cudos to Ethereum by running the command:
```
./gbt --address-prefix="cudos" client cosmos-to-eth \
    --amount="<amount in acudos>" \ # example "1acudos"
    --cosmos-grpc="http://<ip of your cudos node>:9090" \
    --cosmos-phrase="<mnemonic of sender>" \
    --eth-destination="<destination eth address>" \
    --fees="<fee that will be kept in the bridged>"
```

Note that The commands of sending funds takes up to few minutes to be executed.

## Secure your node

Setting up a Cudos Node is the starting point for any user wanting to interact with, and play a greater part in, the network. In order to set up a Cudos node, users will require the use of Go/Golang version 1.15 or higher. On-premise or bare metal server providers such as OVH, Leaseweb, IBM, Alibaba, Amazon Web Services, Google Cloud Computing platform, or Microsoft Azure, can be used to generate Cudos nodes and join the Cudos Network.

Once the appropriate hardware and software requirements are met, users will then need to install the Cudos Network’s high-performance compute blockchain through version control systems such as Github or use the network’s release tags and build from source. The Cudos Network application is the application that defines the Cudos Network and its special purpose compute workflows.

This application consists of a Cudos Network daemon and command-line interface that, once deployed, runs a full-node version of the Cudos Network blockchain for users to interact with. This bespoke implementation supports innovations such as Inter-Blockchain Communication (IBC) protocol to guarantee high levels of reliability and cross-chain interactions inspired by the network’s computing capabilities. This Cudos Network blockchain additionally leverages the most vetted modules within the Cudos community such as staking, authentication, and governance logic. It also includes special blockchain components and developer toolchains linked to its unique set of high-performance compute use cases and development workflows.

### Recommendations for securing a Validator node

Cudos Validating nodes are Cosmos SDK-specified full nodes. This allows for a heavier-duty set of transaction processing workflows and network-level security exchanges with other members of the network. When setting up a Cudos Network Validator node, Validators will have to decide whether they want to be fully responsible for Key Management or if they want to leverage third-party infrastructure to do so. The Cudos Network blockchain leverages the Tendermint Key Management System in order to ensure high availability access to signing keys as part of the Cudos Network’s block processing workflows. This additionally allows this blockchain to prevent double-signing events. In practice, this feature allows for the tamper-proof storage of Hardware Security Module (HSM) Validator keys, even if the host has been compromised. If choosing to implement an HSM, the Validator should review that HSM's documentation in conjunction with the Tendermint KMS requirements, to ensure suitable compatibility before finalising a choice about this part of the security architecture.

An additional security and availability consideration is the use of one or more sentry nodes between a Validator node and the public internet. This mechanism is used to place a layer of separation between the security-sensitive Validator node, and would-be sources of attack.

Such attack types can include things like traffic-volume-based distributed denial-of-service (DDoS) attacks, designed to starve the Validator of available bandwidth in servicing legitimate usage, as well as malformed/spam message attacks, intended to consume processing or storage resources on the Validator, or induce unintended behaviour.

We currently recommend the use of one sentry node whose exterior faces the public internet, with its interior attached to a private internal network. The Validator node then is connected solely to the private internal network, therefore allowing the proxying of all requests to and from the Validator via the sentry. Additionally, we recommend that the sentry's internet connection is further protected by a provider-managed firewall and DDoS-mitigation service.

![](./Security.png)

As previously indicated in the node minimum hardware requirements, our favoured recommendation is to implement discrete physical servers for the Validator and sentry. Using that specification, the separation of public internet and private internal network may be achieved either through physically separated interfaces, or a VLAN-based configuration.

It is technically possible to use a hypervisor on a single physical server, with system VMs for each of the Validator and sentry roles, and to use virtual networking to create the recommended security topology. We do not discourage this approach, but do strongly recommend a thorough understanding of the prospective security and performance considerations prior to implementation.

An extension of the sentry node architecture optionally sees a Validator operator adding additional sentries. For those concerned about risks such as DDoS attacks consuming all bandwidth into the single recommended sentry, it is possible to add further sentries, optionally across multiple discrete geographies with independent internet connections. The private internal network for connection between sentries and the Validator would then need to be stretched out to these.

Beyond the set up of a server, a node, an authenticated way of joining the Cudos blockchain using our in-built public key infrastructure, in coordination with Ledger HSM or YubiHSM for those Validators choosing to implement them, the use of full nodes when interacting the network is highly recommended. We plan to implement the ability for Cudos Validator Nodes to store a history of previously signed blocks in order to more seamlessly prevent double-signing by adverse or deficient nodes in the Cudos Network. This feature is currently absent in earlier-generation Tendermint blockchains. The final element keeping Cudos Network Validating nodes safe is the Tendermint Core Byzantine Fault Tolerant Proof of Stake consensus algorithm.

## Hardware Requirements

Root node in network of 3 validators:

- AMD Rome 2 vCPUs, 4 GB memory
- Cudos-root-node-disk - 320 SSD
- CPU Utilization: ~5%
- Memory Utilization: ~15%
- Network Utilization: ~5KiB/s
- Disk: ~ 3 GB for 31 days in current environment

The following are the minimum hardware requirements. Validators will need to run to successfully join and operate on the Cudos Network for the Testnet and Mainnet (Version 1) stage:

### Validator node

- CPU: 6vCPUs and above at ≥2.0 GHz
- Memory: above 16GiB ECC system memory
- Network:  1Gb/s or 10Gb/s internal network for peer node connections
- Storage: ≥1TB NVMe SSD - RAID1 or better resilience required for the first year

### Sentry node

- CPU: 4vCPUs and above at ≥2.0 GHz
- Memory: above 8GiB ECC system memory
- Network:  1Gb/s internet connection, but 100 MB/s will do
- Storage: ≥1TB NVMe SSD for the first year (Same data as root)

### Seed node

- CPU: 4vCPUs and above at ≥2.0 GHz
- Memory: above 8GiB ECC system memory
- Network:  1Gb/s internet connection, but 100 MB/s will do
- Storage: ≥1TB NVMe SSD for the first year (Same data as root)

### Ethereum node

- CPU: 4vCPUs and above at ≥2.0 GHz
- Memory: above 16GiB ECC system memory
- Network:  1Gb/s internet connection, but 100 MB/s will do
- Storage: ≥500GB NVMe SSD for the first year