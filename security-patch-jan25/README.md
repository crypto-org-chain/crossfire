# Security Patch

## WARNING
This is **NOT** part of the [Network Upgrade](https://blog.crypto.com/crypto-com-chain-crossfire-mainnet-dry-run-details#network-upgrade) tasks in the competition but an important security patch that requires immediate attention. All participants are recommended to upgrade to this version immediately to reduce the impact of the potential security issue.

## Details

A security patch is released on 25 January which based on the patched release of Cosmos SDK 0.40.1 that includes security fixes in Tendermint 0.40.3.

https://github.com/crypto-com/chain-main/releases/tag/v0.8.1-crossfire

## How to upgrade?

- [Running on Your Hosted Machine](#running-on-your-hosted-machine)
- [1-click Deployment](#1-click-deployment)

### Running on Your Hosted Machine

1. Download the latest binary from [GitHub](https://github.com/crypto-com/chain-main/releases/tag/v0.8.1-crossfire). Below is an example if you are running on a **Linux** machine:
    ```bash
    $ curl -LOJ  https://github.com/crypto-com/chain-main/releases/download/v0.8.1-crossfire/chain-main_0.8.1-crossfire_Linux_x86_64.tar.gz
    tar -zxvf chain-main_0.8.1-crossfire_Linux_x86_64.tar.gz
    ```
    You should see the `chain-maind` executable file on your current folder.
1. Stop your Crypto.com Chain node
    - System Service
        ```bash
        $ sudo systemctl stop chain-maind.service
        ```
1. Replace your old `chain-maind` with the new updated version from step 1
1. Verify the version of your `chain-maind` is correct
    ```bash
    $ ./chain-maind version
    0.8.1-crossfire
    ```
1. Start your Crypto.com Chain node again
    - System Service
        ```bash
        $ sudo systemctl start chain-maind.service
        ```

### 1-Click Deployment

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
1. Replace your old `chain-maind` with the new updated version from step 1
    ```bash
    $ sudo mv chain-maind /chain/bin/chain-maind
    ```
1. Verify the version of your `chain-maind` is correct
    ```bash
    $ /chain/bin/chain-maind version
    0.8.1-crossfire
    ```
1. Start your Crypto.com Chain node again
    ```bash
    $ sudo systemctl start chain-maind.service tmkms.service
    ```
1. Check the log of your service
    ```bash
    $ journalctl -u chain-maind.service -f
    ```
