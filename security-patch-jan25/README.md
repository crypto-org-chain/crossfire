# Security Patch on January 25

## WARNING
This is **NOT** part of the [Network Upgrade](https://blog.crypto.com/crypto-com-chain-crossfire-mainnet-dry-run-details#network-upgrade) tasks in the competition but an important security patch that requires immediate attention. All participants are required to upgrade to this version immediately to reduce the impact of the potential security issue.

## Details

A security patch is released on 25 January which based on the patched release of Cosmos SDK 0.40.1 that includes security fixes in Tendermint 0.40.3.

https://github.com/crypto-com/chain-main/releases/tag/v0.8.1-crossfire

## How to upgrade?

- [Running on Your Hosted Machine](#running-on-your-hosted-machine)
- [1-click Deployment](#1-click-deployment)

## Running on Your Hosted Machine

1. Download the latest binary from [GitHub](https://github.com/crypto-com/chain-main/releases/tag/v0.8.1-crossfire). Below is an example if you are running on a **Linux** machine:
    ```bash
    $ curl -LOJ  https://github.com/crypto-com/chain-main/releases/download/v0.8.1-crossfire/chain-main_0.8.1-crossfire_Linux_x86_64.tar.gz
    tar -zxvf chain-main_0.8.1-crossfire_Linux_x86_64.tar.gz
    ```
    You should see the `chain-maind` executable file on your current folder.
1. Stop your Crypto.com Chain node:
    - System Service
        ```bash
        $ sudo systemctl stop chain-maind.service
        ```
1. Wait for a minute before the service is stopped completely:
    - System Service
        ```bash
        $ sudo systemctl status chain-maind.service
        ● chain-maind.service - Chain-maind
        Loaded: loaded (/lib/systemd/system/chain-maind.service; enabled; vendor preset: enabled)
        Active: inactive (dead) since Mon 2021-01-25 13:20:14 UTC; 8s ago
        ```
        You should see `Active: inactive (dead)` if it is stopped
1. Replace your old `chain-maind` with the new updated version from step 1
    ```bash
    mv chain-maind [target-chain-maind-location]
    ```
1. Verify the version of your `chain-maind` is correct:
    ```bash
    $ chain-maind version
    0.8.1-crossfire
    ```
1. Remove address book information. Replace `~/.chain-maind` with your node home directory if you are using a different one:
    ```bash
    $ rm ~/.chain-maind/config/addrbook.json
    ```
1. Increase your memory pool size:
    1. Open `~/.chain-maind/config/config.toml` with an editor. Replace `~/.chain-maind` with your node home directory if you are using a different one.
    1. Look for the tag `[mempool]`
    1. Update the `size` to `20000`:
        ```toml
        # Maximum number of transactions in the mempool
        size = 20000
        ```
1. Start your Crypto.com Chain node again:
    - System Service
        ```bash
        $ sudo systemctl start chain-maind.service
        ```

## 1-Click Deployment

1. Download the latest binary from [GitHub](https://github.com/crypto-com/chain-main/releases/tag/v0.8.1-crossfire). Below is an example if you are running on a **Linux** machine:
    ```bash
    $ curl -LOJ  https://github.com/crypto-com/chain-main/releases/download/v0.8.1-crossfire/chain-main_0.8.1-crossfire_Linux_x86_64.tar.gz
    tar -zxvf chain-main_0.8.1-crossfire_Linux_x86_64.tar.gz
    ```
    You should see the `chain-maind` executable file on your current folder.
1. Stop your Crypto.com Chain node
    ```bash
    $ sudo systemctl stop chain-maind.service tmkms.service
    ```
1. Verify the service is stopped completely.
    ```bash
    $ sudo systemctl status chain-maind.service
    ● chain-maind.service - Chain-maind
    Loaded: loaded (/lib/systemd/system/chain-maind.service; enabled; vendor preset: enabled)
    Active: inactive (dead) since Mon 2021-01-25 13:20:14 UTC; 8s ago
    ...

    $ sudo systemctl status tmkms.service
    ● tmkms.service - Tendermint KMS
     Loaded: loaded (/lib/systemd/system/tmkms.service; enabled; vendor preset: enabled)
     Active: inactive (dead) since Mon 2021-01-25 13:37:40 UTC; 2min 40s ago
    ...
    ```
    You should see `Active: inactive (dead)` if it is stopped
1. Replace your old `chain-maind` with the new updated version from step 1
    ```bash
    $ sudo mv chain-maind /chain/bin/chain-maind
    ```
1. Verify the version of your `chain-maind` is correct
    ```bash
    $ /chain/bin/chain-maind version
    0.8.1-crossfire
    ```
1. Remove address book information:
    ```bash
    $ sudo rm /chain/.chain-maind/config/addrbook.json
    ```
1. Increase your memory pool size:
    ```bash
    $ sudo sed -i.bak -E 's#^(size[[:space:]]+=[[:space:]]+).*$#\120000#' /chain/.chain-maind/config/config.toml
    ```
1. Start your Crypto.com Chain node again
    ```bash
    $ sudo systemctl start chain-maind.service tmkms.service
    ```
1. Check the log of your service
    ```bash
    $ journalctl -u chain-maind.service -f
    $ journalctl -u tmkms.service -f
    ```
