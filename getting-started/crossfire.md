# Crossfire: Running Nodes

This is detailed documentation for setting up a Validator or a full node on Crypto.com Crossfire mainnet dry run.

### Supported OS

We officially support macOS, Windows and Linux only. Other platforms may work but there is no guarantee. We will extend our support to other platforms after we have stabilized our current architecture.

### Prepare your machine

To run Crypto.com Chain nodes, you will need a machine with the following minimum requirements:

- Dual-core, x86_64 architecture processor;
- 4GB RAM;
- 100GB of storage space.

## Step 1 - Get the Crypto.com Chain Crossfire binary

We will be using **Linux** for illustration. Binary for
[Mac](https://github.com/crypto-com/chain-main/releases/download/v0.8.0-crossfire/chain-main_0.8.0-crossfire_Darwin_x86_64.tar.gz) and [Windows](https://github.com/crypto-com/chain-main/releases/download/v0.8.0-crossfire/chain-main_0.8.0-crossfire_Windows_x86_64.zip) are also available. See also the [release notes](https://github.com/crypto-com/chain-main/releases/tag/v0.8.0-crossfire).

- To install Crypto.com Chain released binaries from github:

  ```bash
  $ curl -LOJ https://github.com/crypto-com/chain-main/releases/download/v0.8.0-crossfire/chain-main_0.8.0-crossfire_Linux_x86_64.tar.gz
  $ tar -zxvf chain-main_0.8.0-crossfire_Linux_x86_64.tar.gz
  ```

_Remarks for macOS user_: You might have to go to the `Security and Privacy` to approve the execution of `chain-maind`.

- Before proceeding, make sure you are running the crossfire binary (`0.8.0-crossfire`) by checking the version of `chain-maind`:
  ```bash
  $ ./chain-maind version
  0.8.0-crossfire
  ```

## Step 2. Configure `chain-maind`

Before kick-starting your node, we will have to configure your node so that it connects to the Crossfire network:

### Step 2-1 Initialize `chain-maind`

- First of all, you can initialize chain-maind by:

  ```bash
    $ ./chain-maind init [moniker] --chain-id crossfire
  ```

  This `moniker` will be the displayed id of your node when connected to Crypto.com Chain network.
  When providing the moniker value, make sure you drop the square brackets since they are not needed.
  The example below shows how to initialize a node named `pegasus-node` :

  ```bash
    $ ./chain-maind init pegasus-node --chain-id crossfire
  ```

  ::: tip NOTE

  - Depending on your chain-maind home setting, the chain-maind configuration will be initialized to that home directory. To simply the following steps, we will use the default chain-maind home directory `~/.chain-maind/` for illustration.
  - You can also put the `chain-maind` to your binary path and run it by `chain-maind`
    :::

### Step 2-2 Configurate chain-maind

- Download the and replace the Croeseid Testnet `genesis.json` by:

  ```bash
  $ curl https://raw.githubusercontent.com/crypto-com/testnets/main/crossfire/genesis.json > ~/.chain-maind/config/genesis.json
  ```

- Verify sha256sum checksum of the downloaded `genesis.json`. You should see `OK!` if the sha256sum checksum matches.

  ```bash
  $ if [[ $(sha256sum ~/.chain-maind/config/genesis.json | awk '{print $1}') = "074d99565111844edf1e9eb62069b7ad429484c41adcab1062447948b215c3c8" ]]; then echo "OK"; else echo "MISMATCHED"; fi;

  OK!
  ```

- For Cosmos configuration, in `~/.chain-maind/config/app.toml`, update minimum gas price to avoid [transaction spamming](https://github.com/cosmos/cosmos-sdk/issues/4527)

  ```bash
  $ sed -i.bak -E 's#^(minimum-gas-prices[[:space:]]+=[[:space:]]+)""$#\1"0.025basetcro"#' ~/.chain-maind/config/app.toml
  ```

TODO:UPDATE SEED

- For network configuration, in `~/.chain-maind/config/config.toml`, please modify the configurations of `persistent_peers` and `create_empty_blocks_interval` by:

  ```bash
  $ sed -i.bak -E 's#^(persistent_peers[[:space:]]+=[[:space:]]+).*$#\1" [SEED] "# ; s#^(create_empty_blocks_interval[[:space:]]+=[[:space:]]+).*$#\1"5s"#' ~/.chain-maind/config/config.toml
  ```

  ::: tip STATE-SYNC
  [STATE-SYNC](https://docs.tendermint.com/master/tendermint-core/state-sync.html) is supported in Crossfire! 🎉

With state sync your node will download data related to the head or near the head of the chain and verify the data. This leads to drastically shorter times for joining a network for validator.

However, you should keep in mind that the block before state-sync `trust height` will not be queryable. So if you want to run a full node, better not use state-sync feature to ensure your node has every data on the blockchain network.
For validator, it will be amazingly fast to sync the near head of the chain and join the network.

Follow the below optional steps to enable state-sync.
:::

- (Optional) For state-sync configuration, in `~/.chain-maind/config/config.toml`, please modify the configurations of [statesync] `enable`, `rpc_servers`, `trust_height` and `trust_hash` by:
  Also, we suggest using `persistent_peers` instead of `seeds` to to provide stable state-sync experience.

  ```bash
  $ LASTEST_HEIGHT=$(curl -s https://crossfire.crypto.com/block | jq -r .result.block.header.height); \
  BLOCK_HEIGHT=$((LASTEST_HEIGHT - 1000)); \
  TRUST_HASH=$(curl -s "https://crossfire.crypto.com/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

  $ sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
  s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"https://crossfire.crypto.com:443,https://crossfire.crypto.com:443\"| ; \
  s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
  s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
  s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" ~/.chain-maind/config/config.toml
  ```

## Step 3. Run everything

::: warning CAUTION
This page only shows the minimal setup.

You may want to run full nodes
as sentries (see [Tendermint](https://docs.tendermint.com/master/tendermint-core/running-in-production.html)), restrict your validator connections to only connect to your full nodes, test secure storage of validator keys etc.
:::

### Step 3-1. Create a new key and address

Selected [participants](https://github.com/crypto-com/crossfire/blob/main/testnet-participants.json) would have a balance of `100,000` tcro in their address, kindly contact with the stuff at the Crossfire channel (under #crossfire): [![Support Server](https://img.shields.io/discord/783264383978569728.svg?color=7289da&label=Crypto.com-Chain&logo=discord&style=flat-square)](https://discord.gg/pahqHz26q4) if that's not the case. 


Using the [mnemonic](https://github.com/crypto-com/crossfire/blob/main/assets/create-address.md#step-2---create-a-new-key-and-address-for-crossfire) created earlier, you can restore an existing key by.

```bash
$ chain-maind keys add [Key_name] --recover 
> Enter your bip39 mnemonic
## Enter your 24-word mnemonic here ##
```

You should obtain an address with `cro` prefix, e.g. `cro1quw5r22pxy8znjtdkgqc65atrm3x5hg6vycm5n`. This will be the address for performing transactions.

Before proceeding, make sure you can list the key: We should be able to see the key while using the keys list command, for example:

```bash
$ ./chain-maind keys list
- name: [Key_name]
type: local
address: cro1uayxmr8597lfg27q2zngphnmdwjnkdhadsdc3x
pubkey: cropub1addwnpepqt255g89h57ttkk2sdklsqn5pcc3z8fecrpe2rh3yka5xn484lx8k6gs8t6
mnemonic: ""
threshold: 0
pubkeys: []
```

### Step 3-2. Obtain the validator public key

You can obtain your validator public key by:

```bash
  $ ./chain-maind tendermint show-validator
```

The public key should begin with the `crocnclconspub1` prefix, e.g. `crocnclconspub1zcjduepq7xn9ne5zkdldexy3xnuu3leqqxq0utzw06u8xgpurwp7muvv8gyq3aehvx`.

### Step 3-3. Run everything

Once the `chain-maind` has been configured, we are ready to start the node and sync the blockchain data:

- Start chain-maind, e.g.:

```bash
  $ ./chain-maind start
```

- _(Optional for Linux)_ Start chain-maind with systemd service, e.g.:

```bash
  $ git clone https://github.com/crypto-com/chain-main.git && cd chain-main
  $ ./networks/create-service.sh
  $ sudo systemctl start chain-maind
  # view log
  $ journalctl -u chain-maind -f
```

:::details Example: /etc/systemd/system/chain-maind.service created by script

```bash
# /etc/systemd/system/chain-maind.service
[Unit]
Description=Chain-maind
ConditionPathExists=/usr/local/bin/chain-maind
After=network.target

[Service]
Type=simple
User=ubuntu
WorkingDirectory=/usr/local/bin
ExecStart=/usr/local/bin/chain-maind start --home /home/ubuntu/.chain-maind
Restart=on-failure
RestartSec=10
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
```

:::

It should begin fetching blocks from the other peers. Please wait until it is fully synced before moving onto the next step.

For example, one can check the current block height by querying the public full node by:

```bash
curl -s https://crossfire.crypto.com/commit | jq "{height: .result.signed_header.header.height}"
```

### Step 3-4. Send a `create-validator` transaction

Once the node is fully synced, we are now ready to send a `create-validator` transaction and join the network, for example:

```
$ ./chain-maind tx staking create-validator \
--from=[name_of_your_key] \
--amount=50000tcro \
--pubkey=[crocnclconspub...]  \
--moniker="[The_id_of_your_node]" \
--security-contact="[security contact email/contact method]" \
--chain-id="crossfire" \
--commission-rate="0.10" \
--commission-max-rate="0.20" \
--commission-max-change-rate="0.01" \
--min-self-delegation="1" \
--gas 80000000 \
--gas-prices 0.1basetcro

{"body":{"messages":[{"@type":"/cosmos.staking.v1beta1.MsgCreateValidator"...}
confirm transaction before signing and broadcasting [y/N]: y
```

You will be required to insert the following:

- `--from`: The `cro...` address that holds your funds;
- `--pubkey`: The validator public key( See Step [3-2](#step-3-2-obtain-the-a-validator-public-key) above ) with **crocnclconspub** as the prefix;
- `--moniker`: A moniker (name) for your validator node. **NOTE: Kindly stick to the moniker you have provided earlier in the email**;
- `--security-contact`: Security contact email/contact method.

Once the `create-validator` transaction completes, you can check if your validator has been added to the validator set:

```bash
$ ./chain-maind tendermint show-address
## [crocnclcons... address] ##
$ ./chain-maind query tendermint-validator-set | grep -c [crocnclcons...]
## 1 = Yes; 0 = Not yet added ##
```

To further check if the validator is signing blocks, kindly run this [script](https://github.com/crypto-com/chain-docs/blob/master/docs/getting-started/assets/signature_checking/check-validator-up.sh), for example:

```bash
$ curl -sSL https://raw.githubusercontent.com/crypto-com/chain-docs/master/docs/getting-started/assets/signature_checking/check-validator-up.sh | bash -s -- \
--tendermint-url https://crossfire.crypto.com \
--pubkey $(cat ~/.chain-maind/config/priv_validator_key.json | jq -r '.pub_key.value')

The validator is in the active validator set under the address  <YOUR_VALIDATOR_ADDRESS>
The validator is signing @ Block#<BLOCK_HEIGHT> 👍
```

For those who are using tmkms in background([AWS](./testnet-aws-1click.html)/[Azure](./testnet-azure-1click.html) 1-click deployment are using tmkms), you should use `--bechpubkey` the consensus pubkey with prefix `crocnclconspub1....` directly instead of `--pubkey` tendermint pubkey since the node is not using `~/.chain-maind/config/priv_validator_key.json` for signing.

```bash
$ curl -sSL https://raw.githubusercontent.com/crypto-com/chain-docs/master/docs/getting-started/assets/signature_checking/check-validator-up.sh | bash -s -- \
--tendermint-url https://crossfire.crypto.com \
--bechpubkey [crocnclconspub1....]

The validator is in the active validator set under the address  <YOUR_VALIDATOR_ADDRESS>
The validator is signing @ Block#<BLOCK_HEIGHT> 👍
```

Alternatively, you can run it on this [browser based IDE](https://repl.it/@allthatjazzleo/cryptocomcheckNodeJoinStatus#main.go), by specifying your validator public key in the `"YOUR_PUBKEY"` field, where this key can be obtained by running

```bash
$ cat ~/.chain-maind/config/priv_validator_key.json | jq -r '.pub_key.value'
```

## Step 4. Perform Transactions

### Step 4-1. `query bank balances` - Check your transferable balance

You can check your _transferable_ balance with the `balances` command under the bank module.
:::details Example: Check your address balance

```bash
$ ./chain-maind query bank balances cro12rkg6u2pyp08n29p0egtjdynmpu3t75pdd94ed

balances:
- amount: "10000000000000"
  denom: basetcro
pagination:
  next_key: null
  total: "0"

```

:::

### Step 4-2. `tx bank send ` - Transfer operation

Transfer operation involves the transfer of tokens between two addresses.

#### **Send Funds** [`tx bank send <from_key_or_address> <to_address> <amount> <network_id>`]

:::details Example: Send 10tcro from an address to another.

```bash
$ chain-maind tx bank send Default
cro1j7pej8kplem4wt50p4hfvndhuw5jprxxn5625q 10tcro --chain-id "crossfire"
  ## Transaction payload##
  {"body":{"messages":[{"@type":"/cosmos.bank.v1beta1.MsgSend","from_address"....}
confirm transaction before signing and broadcasting [y/N]: y
```

:::

### Step 4-3. `tx staking ` - Staking operations

Staking operations involve the interaction between an address and a validator. It allows you to create a validator and lock/unlocking funds for staking purposes.

#### **Delegate you funds to a validator** [`tx staking delegate <validator-addr> <amount>`]

To bond funds for staking, you can delegate funds to a validator by the `delegate` command

::: details Example: Delegate funds from `Default` to a validator under the address `tcrocncl16k...edcer`

```bash
$ chain-maind tx staking delegate crocncl16kqr009ptgken6qsxnzfnyjfsq6q97g3uedcer 100tcro --from Default --chain-id "crossfire"
## Transactions payload##
{"body":{"messages":[{"@type":"/cosmos.staking.v1beta1.MsgDelegate"....}
confirm transaction before signing and broadcasting [y/N]: y
```

:::

#### **Unbond your delegated funds** [`tx staking unbond <validator-addr> <amount>`]

On the other hand, we can create a `Unbond` transaction to unbond the delegated funds

::: details Example: Unbond funds from a validator under the address `tcrocncl16k...edcer`

```bash
$ chain-maind tx staking unbond crocncl16kqr009ptgken6qsxnzfnyjfsq6q97g3uedcer 100tcro --from Default --chain-id "crossfire"
## Transaction payload##
{"body":{"messages":[{"@type":"/cosmos.staking.v1beta1.MsgUndelegate"...}
confirm transaction before signing and broadcasting [y/N]: y
```

:::

::: tip

- Once your funds were unbonded, It will be locked until the `unbonding_time` has passed.

:::

Congratulations! You've successfully setup a node and performed some of the basic transactions! You may refer to [Wallet Management](https://chain.crypto.com/docs/wallets/cli.html#chain-maind) for more advanced operations and transactions.

