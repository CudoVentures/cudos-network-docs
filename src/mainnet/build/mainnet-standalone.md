---
title: Standalone Node Build
---

# Standalone Node Build

## Initialize and start the Node
This section explains how to initialize and start a standalone full node for testing purposes. 

**Please ensure you have completed everything in [Node Environment Preparation](/mainnet/build/mainnet-envprep.html) before you continue to create your node.**


#### 1. As root, navigate to the directory `/var/lib/cudos/CudosBuilders/docker/full-node`
```
sudo -i
cd /var/lib/cudos/CudosBuilders/docker/full-node
```
#### 2. Create a copy of `full-node.env.example`, naming the copy `<tbc>.env`
```
cp full-node.env.example full-node.client.testnet.public01.env
```
#### 3. Open the file `<tbc>.env`. 

- Set the `"MONIKER"` (your node’s name on the blockchain) attribute to your desired name:
```
MONIKER=<your-node-moniker>
```
- Set the flag "SHOULD_USE_GLOBAL_PEERS" to true :
```
SHOULD_USE_GLOBAL_PEERS=true
```
Exit and Save the file

#### 4. Make sure that you are still in the correct directory `/var/lib/cudos/CudosBuilders/docker/full-node`, and *Initialize* the node by running this command:
```
docker-compose --env-file <tbc>.arg -f <tbc>.yml -p <tbc> up --build
```

#### 5. *Start* your node
```
docker-compose --env-file <tbc>.arg -f <tbc>.yml -p <tbc> up --build --detach
```


If all steps are completed successfully, you should see a newly generated file: 
`/var/lib/cudos/CudosData/<tbc>/tendermint.nodeid`
that contains your node ID, consisting of a long string of random characters.

::: tip
Syncing may take several hours. Refer to [Checking sync status](/mainnet/sync-troubleshooting.html) to verify your node is syncing. 
::: 

 
Once your node is synced, you will be able to either:
- Use it as a **Full Node**. If you wish to send transactions from your node, you will need to create a wallet, fund it, and stash it in your node. Refer to [Setting up your nodes wallet](/mainnet/build/mainnet-fundnodes.html) for instructions.
- Configure it as a **Standalone Validator Node:**

## Standalone Validator Node
### Staking
The standalone Validator node consists of a single full node running as a validator. 

A full node becomes a validator when it has successfully staked CUDOS.

Before staking, you must create and fund your wallet, and stash it on your node by following the process described in [Setting up your nodes wallet](/mainnet/build/mainnet-fundnodes.html), making sure to use the Mainnet `cudos-1`.

Once this is completed, you can go ahead and stake CUDOS, which will make your node a validator, as described in [Staking your Validator](/mainnet/build/mainnet-fundnodes.html#staking-your-validator), making sure to use theMainnet `cudos-1`.