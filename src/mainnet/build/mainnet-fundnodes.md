---
title: Funding your nodes
---

# Funding your nodes

## Setting up your nodes wallet
In order to sign transactions, pay gas and (if it is a validator) stake CUDOS on your node, you will need to assign a wallet to it. The following instructions take you through:

- [Creating](#create-a-keplr-wallet) your wallet 
- [Funding](#funding-your-wallet) your wallet
- [Stashing](#stashing-the-wallet-keys) your wallet key and address


### Create a Keplr Wallet

#### What is Keplr

[Keplr](https://wallet.keplr.app/#/dashboard) is a browser extension wallet that supports the Cudos blockchain. It provides functionalities for account and wallet management.

[Keplr extension](https://chrome.google.com/webstore/detail/keplr/dmkamcknogkgcdfhhbddcghachkejeap?hl=en) is supported only on Chrome. It can also be used on Brave, but some features may be unavailable. You can not use the Keplr extension from your mobile. Please note that anyone with your mnemonic seed can take your assets and any lost mnemonic seed can not be recovered.

#### Installing Keplr

Use Keplr extension to create a wallet:
1. Install the [Keplr extension](https://chrome.google.com/webstore/detail/keplr/dmkamcknogkgcdfhhbddcghachkejeap?hl=en) on your browser.
2. Click on the installed Keplr extension, and either select ‘Create a new account’ or ‘Import an already existing account’. Note that currently the wallet is not connected to the cudos-network, so it will not show the correct info for that account on the cudos-network.
3. Take a note of your mnemonic and store it in a safe place.

::: danger
The mnemonic of the keplr wallet is a human readable representation of the wallet’s address and key combined. **Anyone with your mnemonic can take your assets and any lost mnemonic can not be recovered.**
:::

4. Enter your mnemonic and click Register

#### Link Keplr to the Cudos network

1. Go to the [Cudos Explorer](https://explorer.cudos.org) to link the Keplr extension to the Cudos network
2. Click the "key icon” in the upper right corner

3. This will prompt a window asking you for permission to add a new network to Keplr and also give access to it.



4. After you Approve, open the Keplr extension and click on the network name on the upper side of it. From there a menu with all the networks will open, and you should select  **cudos-1**. 


You should now see your account details.

<!---### Resources

- [Full Guide on How to use Keplr Wallet](https://medium.com/chainapsis/how-to-use-keplr-wallet-40afc80907f6)
- [Keplr Documentation](https://docs.keplr.app/)
- [Introducing Keplr](https://medium.com/everett-protocol/introducing-keplr-an-interchain-wallet-for-cosmos-applications-a260aac64eaa)
- [Keplr FAQ](https://faq.keplr.app/)
--->


### Funding your wallet

A wallet allows you to store and retrieve CUDOS tokens. When you buy or receive CUDOS tokens, you can keep them in a wallet from where you can fund transactions, pay gas, or stake CUDOS as a validator or delegator.

This article explains where to buy/trade CUDOS and how to apply for grants.

#### Where to buy CUDOS?

The current top exchanges for CUDOS trading are BitMax, KuCoin, Gate.io, Uniswap, and Poloniex. You can also trade CUDOS on the following exchanges:

|**Exchange**|**Available Trading Pair**|
| - | - |
|[Gate.io](https://www.gate.io/)|[CUDOS/USDT](https://gate.io/trade/cudos_usdt)|
|[KuCoin](https://www.kucoin.com/)|[CUDOS/USDT](https://trade.kucoin.com/CUDOS-USDT)|
|[AscendEX (Bitmax)](https://www.ascendex.com/)|[CUDOS/USDT](https://www.ascendex.com/en/basic/cashtrade-spottrading/usdt/cudos)|
|[1inch Exchange](https://1inch.io/)|[ETH/CUDOS](https://app.1inch.io/#/ETH/CUDOS), [CUDOS/ETH](https://app.1inch.io/#/CUDOS/ETH)|
|[Bilaxy](https://bilaxy.com/)|[CUDOS/USDT](https://bilaxy.com/trade/CUDOS_USDT)|
|[Poloniex](https://bilaxy.com/)|[CUDOS/USDT](https://poloniex.com/exchange/USDT_CUDOS)|
|[0x Protocol](https://matcha.xyz/)|
|[Uniswap (V2)](https://uniswap.org/)|[CUDOS/WETH](https://app.uniswap.org/#/swap?inputCurrency=0x817bbDbC3e8A1204f3691d14bB44992841e3dB35&outputCurrency=0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2)|

For more information:

- [How to Buy CUDOS On Poloniex — A Step by Step Guide](https://cryptobuyingtips.medium.com/60-92-growth-how-to-buy-cudos-cudos-a-step-by-step-guide-crypto-buying-tips-16e9a022bb6a)
- [How To Buy CUDOS On Uniswap A Step-by-Step Guide](https://www.pickacrypto.com/how-to-buy-cudos-token/)


#### Apply for grants

The Cudos grants program offers funding for various opportunities:

- Application development
- Infrastructure, tools and APIs
- Research and community

The grant program aims to support the growth of the Cudos ecosystem. This will empower innovation and development and provide new opportunities for researchers, developers, and the community.

Please submit your application or proposal through this [form](https://www.cudos.org/#contact-us).

You can read the [Foundation & Grants](/earn/grants.html) section for more information.

### Stashing the wallet Keys

In order to sign transactions, pay gas, or stake coins, first you must stash your wallet key and address on a keyring on your node.

:::  tip
The process for stashing a key on a full node or a validator is the same, however you should use a meaningful string as the name for your keyring; for example  `validator1keyring` if the wallet is going to be used by a validator, or `fullnode1keyring` if it is for a full node.
:::

To carry out this procedure you will need the **mnemonic** of the Keplr wallet that you want to use with the node. The mnemonic of the keplr wallet is a human readable representation of the wallet’s address and key combined, consisting of a string of random words. It was created when you first created your wallet.
You will also be asked to enter a new **passphrase** for your keyring. From then on you will access the keyring with that passphrase.
 
In the following example, a wallet’s mnemonic is added to keyring `validator1keyring`.
 
Enter the docker container `cudos-start-full-node-mainnet`:

```
sudo docker exec -it cudos-start-full-node-mainnet bash
 ```

To add your wallet’s mnemonic to the keyring `validator1keyring`, enter the following command:

```
cudos-noded keys add validator1keyring --recover --keyring-backend="os"
```

You will be prompted to enter the mnemonic of the Keplr wallet

```
> Enter your bip39 mnemonic
```

Then you will be prompted to create your keyring passphrase by entering it twice. **Take a note of your passphrase as you will need it to access your keyring in the future:**

```
Enter keyring passphrase:
Re-enter keyring passphrase:
```

::: tip
Once your keyring is created, you can add other wallets to it by repeating the above, the only difference being you will be prompted only once for the passphrase.
:::

You should now see output like the following, which means the process is complete and the keyring `validator1keyring` is now loaded with the wallet address and key:

```
- name: validator1keyring
  type: local
  address: cudos1qr5rt72yf7s340azajpxay6hw3z5ldne7dd5n3
  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"AmeChzeLCPCtPKrIVs7hp737DBNU7XlYVwDZfhJ3SdXq"}'
  mnemonic: ""
```

Your Full Node or validator is now set up to use your Keplr wallet.


## Staking your Validator

This step, if successful, will put the node on the [list of validators](https://explorer.cudos.org/validators).

In this section of the process the validator node will need to be supplied with the staking request using a cudos-noded sub-command that will use the following environment variables:

- CHAIN_ID, This is a fixed text naming the blockchain to be operated on. In the public mainnet this name is `cudos-1`
- STAKE, The actual amount in "acudos" that will be staked to the validator. Note that acodos is a very small denomination. Be very careful about the number of zeros in the amount. For example "1000000000000000000acudos" = 1 CUDOS.
- MONIKER, that you assigned in the validator node’s `/var/lib/cudos/CudosBuilders/docker/full-node/full-node.client.mainnet.env` file.
 
 
Enter your docker container:

```
sudo docker exec -it cudos-start-full-node-mainnet bash
```

This script will stake and self delegate 2 million CUDOS (`2000000000000000000000000acudos`) from the wallet that you linked to your keyring (in this example we will use the keyring `validator1keyring`) on Full node with moniker `Validator1`.You will be prompted to enter the passphrase for keyring `validator1keyring`.


```
export STAKE="2000000000000000000000000acudos"
export CHAIN_ID="cudos-1"
Export MONIKER="Validator1"

cudos-noded tx staking create-validator --amount=$STAKE \
    --from=validator1keyring \
    --pubkey=$(cudos-noded tendermint show-validator) \
    --moniker=$MONIKER \
    --chain-id=$CHAIN_ID \
    --commission-rate="0.10" \
    --commission-max-rate="0.20" \
    --commission-max-change-rate="0.01" \
    --min-self-delegation="2000000000000000000000000" \
    --gas-prices="5000000000000acudos" \
    --keyring-backend="os" \
    -y
 ```

You will be prompted to enter your keyring passphrase.


If successful, you should see a long output with no errors. After a few minutes, if you go to [the Validators tab in Explorer](https://explorer.cudos.org/validators) and you can see your MONIKER in the list of validators then **you have successfully staked on your validator, and it is operational.**

<p>&nbsp;</p>

---

If you get a message similar to:
```
Error: rpc error: code = NotFound desc = rpc error: code = NotFound desc = account cudos1mnc7gm9sazrmcfdkshhmx3f0s4n2wp944wzjj4 not found: key not found`
```
Then it is likely that the validator is still syncing. Refer to [Checking sync status](/mainnet/sync-troubleshooting.html) to verify your node’s sync status. 

If you can’t see your node in the explorer's [Validators tab](https://explorer.cudos.org/validators), check the Inactive tab on the Cudos explorer.
 
### Get the validator’s operator address
If you want to find your validator’s operator address, run the command:

```
cudos-noded q staking validators | grep -B13 -A9 "$MONIKER" | grep operator_address
```


## Changing the stake on your Validator
The process to add or remove stake as a Validator is similar to Delegating CUDOS, however you are Delegating to your own Validator.

###  Add stake to my Validator

1. If you have not done so already, connect your Validator's wallet to the Explorer and navigate to your Validator's page in the explorer:


2. Click **DELEGATE** and introduce the amount you wish to stake in order to increase your Validator's share

3. Click **Next** and approve the transaction in Keplr to increase your Validator's stake.

### Move part of my stake from my Validator to a different Validator

The same process as above, but clicking the **REDELEGATE** button instead and selecting the new Validator which should receive the staked tokens.

### Remove stake from my Validator

Same process again, but clicking the **UNDELEGATE** button.

Please keep in mind that:

- A Validator needs a minimum amount staked (2,000,000 CUDOS for mainnet).
- There is a 21-day unbonding period between token undelegation and the tokens being usable in your wallet, for security reasons.

## Claiming rewards

In order to claim pending rewards with your validator, simply

1. Open your Keplr wallet and click the **Claim** button

2. Approve the transaction, and wait for it to be processed.

## Changing your Validator fee

In order to change your Validator's fee

1. Start your Docker shell

2. Execute the following command:
```
cudos-noded tx staking edit-validator \
--from=validator \
--chain-id=$CHAIN_ID \
--commission-rate="0.50" \
--keyring-backend="test" \
-y
```
where you can set `commission-rate` to the updated number that you want.

You can check your current Validator's fee by running
```
cudos-noded q staking validators > validatorsInfo.txt
```
and reading the value assigned to `commission-rate` for your Validator.
