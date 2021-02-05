# Crossfire V0.9.1 upgrade

## WARNING
This is part of the [Network Upgrade](https://blog.crypto.com/crypto-com-chain-crossfire-mainnet-dry-run-details#network-upgrade) tasks in the competition. All participants are required to upgrade to this version After 05-02-2021, 09:00UTC.



## How to upgrade?

- [Running on Your Hosted Machine](#running-on-your-hosted-machine)
- [1-click Deployment](#1-click-deployment)

## Running on Your Hosted Machine

1. Download the latest binary from [GitHub](https://github.com/crypto-com/chain-main/releases/tag/v0.9.1-crossfire). Below is an example if you are running on a **Linux** machine:
    ```bash
    $ curl -LOJ  https://github.com/crypto-com/chain-main/releases/download/v0.9.1-crossfire/chain-main_0.9.1-crossfire_Linux_x86_64.tar.gz
    tar -zxvf chain-main_0.9.1-crossfire_Linux_x86_64.tar.gz
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
        Active: inactive (dead) since Wed 2021-02-03 13:20:14 UTC; 8s ago
        ```
        You should see `Active: inactive (dead)` if it is stopped
1. Replace your old `chain-maind` with the new updated version from step 1
    ```bash
    mv chain-maind [target-chain-maind-location]
    ```
1. Verify the version of your `chain-maind` is correct:
    ```bash
    $ chain-maind version
    0.9.1-crossfire
    ```

1. Start your Crypto.com Chain node again:
    - System Service
        ```bash
        $ sudo systemctl start chain-maind.service
        ```

## 1-Click Deployment

1. Download the latest binary from [GitHub](https://github.com/crypto-com/chain-main/releases/download/v0.9.1-crossfire/chain-main_0.9.1-crossfire_Linux_x86_64.tar.gz). Below is an example if you are running on a **Linux** machine:
    ```bash
    $ curl -LOJ  https://github.com/crypto-com/chain-main/releases/download/v0.9.1-crossfire/chain-main_0.9.1-crossfire_Linux_x86_64.tar.gz
    tar -zxvf chain-main_0.9.1-crossfire_Linux_x86_64.tar.gz
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
    Active: inactive (dead) since Wed 2021-02-03 13:20:14 UTC; 8s ago
    ...

    $ sudo systemctl status tmkms.service
    ● tmkms.service - Tendermint KMS
     Loaded: loaded (/lib/systemd/system/tmkms.service; enabled; vendor preset: enabled)
     Active: inactive (dead) since Wed 2021-02-03 13:20:14 UTC; 8s ago
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
    0.9.1-crossfire
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
