# Public Testnet Upgrade v0.9.0

The following instructions guide a Validator through the process of upgrade to v0.9.0 of the Cudos Network Public Testnet. The process is the same per node and should be executed as described for every node.

## Prepare The Environment
1. Create a directory for your upgrade scripts
``` bash
mkdir ~/cudosfork090
```
2. Clone the repo:
```bash
cd ~/cudosfork090
git clone --branch v0.9.0 https://github.com/CudoVentures/cudos-builders.git CudosBuilders
```
3. Navigate to ~/cudosfork090/CudosBuilders/tools-bash/upgrade/config 
```bash 
cd ~/cudosfork090/CudosBuilders/tools-bash/upgrade/config 
```
4. Make a copy of `node.env.example` and name it `node.env`
```bash
cp node.env.example node.env
```
5. Open the `node.env` and define the 3 variables.

    <em>**"PARAM_SOURCE_DIR"**</em> must point to the folder that contains <em>"CudosNode"</em>, 
    <em>"CudosBuilders"</em>, <em>"CudosGravityBridge"</em> and <em>"CudosData"</em>. This could be in different places based on your previous setup, some possible examples include:
    `"/root/cudos"`, `"/var/lib/cudos"`, `"/usr/cudos"`

    Set <em>**"PARAM_NODE_NAME"**</em> to the type of node you are updating. Possible values are: seed-node, sentry-node or full-node (for the validator). Example: PARAM_NODE_NAME="sentry-node"
    
    (root-node is also an option for Cudos internal use)
    
    <em>**"PARAM_HAS_ORCHESTRATOR"**</em> should be set to true if you have an orchestrator.

**Example** content of a configuration file by node:
<p>PARAM_SOURCE_DIR value is an example. Please verify you are setting the correct one.</p>

**For full-node:**
```bash
PARAM_SOURCE_DIR="/usr/cudos"
PARAM_NODE_NAME="full-node"
PARAM_HAS_ORCHESTRATOR=""
```

**For seed-node:**
```bash
PARAM_SOURCE_DIR="/usr/cudos"
PARAM_NODE_NAME="seed-node"
PARAM_HAS_ORCHESTRATOR=""
```

**For sentry-node:**
```bash
PARAM_SOURCE_DIR="/usr/cudos"
PARAM_NODE_NAME="sentry-node"
PARAM_HAS_ORCHESTRATOR=""
```

## Launch Sequence

1. Create a backup

    <em>Note:</em> Creating of a backup could take a lot of time. It is very important to do it ONCE upgrade height has been reached NOT before that. Make sure there is no error messages in the console. If something went wrong you can always re-create the backup. Make sure that the backup is correct (You can check it using <em>Validate a backup</em>) before proceeding to the next step. The backup can take around 10-20 minutes - it is a 91GiB transfer.

2. Validate

    <em>Note:</em> The validate command will print the information about current node. Read it carefully and proceed with the next step only if this information is valid. If it is not valid, please contact CUDOS and make the appropriate changes. If the changes invole any of the previously backup-ed files, you must re-create the backup.

3. Upgrade

    <em>Note: </em> The upgrade could take up to 20min. If there is any error message during the upgrade you must restore a backup (using <em>Restore a backup</em>) and start over.
    

::: tip
Execute the scripts only when all config files are ready.

All of the scripts below must be executed from ./upgrade folder.

Make sure that ./src/backup.sh, ./src/node.sh and ./src/gravity.sh have execute permission.

You can do this with:

```bash
chmod 744 ./src/backup.sh
```
```bash
chmod 744 ./src/node.sh
```
```bash
chmod 744 ./src/gravity.sh
```
::: 

## Backup
The backup script has four usages:

### Create a Backup
The command below creates a backup of current source files and data files.
``` bash
cd ~/cudosfork090/CudosBuilders/tools-bash/upgrade
sudo ./src/backup.sh create
```

### Restore a Backup (optional)
The command restores a backup that has been created using Create a backup
``` bash
cd ~/cudosfork090/CudosBuilders/tools-bash/upgrade
sudo ./src/backup.sh restore
```

### Validate a Backup
The command validates whether a created backup using Create a backup is valid
``` bash
cd ~/cudosfork090/CudosBuilders/tools-bash/upgrade
sudo ./src/backup.sh validate
```

### Clean a Backup (optional)
The command deletes previously created backup using [Create a backup](##Create-a-backup)  **DO NOT** use before the node started signing blocks or in an event of emergency. Ideally, leave the backup in place.
``` bash
cd ~/cudosfork090/CudosBuilders/tools-bash/upgrade
sudo ./src/backup.sh clean
```

## Upgrade the Network

## Node

Node script has following usages:

### Validate the Config/Setup
The command check for installed binaries, config files, repos, etc.
```
sudo ./src/node.sh validate
```

### Perform An Upgrade
The command upgrades a node
```
sudo ./src/node.sh upgrade
```

### Perform an Upgrade Bypassing The Exporting and Migrating of the Genesis File (optional)
The command upgrades a node by using an external genesis. This option cannot be used unless the upgraded network has started producing blocks
```
sudo ./src/node.sh upgrade-with-predefined-genesis
```

For informational purposes: please be aware that a necessary part of this upgrade is a change to the Chain ID from `cudos-testnet-public-2` to `cudos-testnet-public-3`, this is common practice for Cosmos-based chains. Note that some old docs may refer to the old Chain ID.
