
## Get the Crypto.com Chain Crossfire binary

We will be using **Linux** for illustration. Binary for
[Mac](https://github.com/crypto-com/chain-main/releases/download/v0.8.0-crossfire/chain-main_0.8.0-crossfire_Darwin_x86_64.tar.gz) and [Windows](https://github.com/crypto-com/chain-main/releases/download/v0.8.0-crossfire/chain-main_0.8.0-crossfire_Windows_x86_64.zip) are also available. See also the [release notes](https://github.com/crypto-com/chain-main/releases/tag/v0.8.0-crossfire).

- To install Crypto.com Chain released binaries from github:

  ```bash
  $ curl -LOJ https://github.com/crypto-com/chain-main/releases/download/v0.8.0-crossfire/chain-main_0.8.0-crossfire_Linux_x86_64.tar.gz
  $ tar -zxvf chain-main_0.8.0-crossfire_Linux_x86_64.tar.gz
  ```

*Remarks for macOS user*: You might have to go to the `Security and Privacy` to approve the execution of `chain-maind`. 


- Before proceeding, make sure you are running the crossfire binary (`0.8.0-crossfire`) by checking the version of `chain-maind`:
    ```bash
    $ ./chain-maind version
    0.8.0-crossfire 
    ```


## Create a new key and address for Crossfire

- Using the crossfire binary, run the following command to create a new key. For example, you can create a key with the name of `cross-fire-testing` by:

    ```bash
    $ ./chain-maind keys add cross-fire-testing  

    Enter keyring passphrase:
    Re-enter keyring passphrase:

    - name: cross-fire-testing
    type: local
    address: cro1uayxmr8597lfg27q2zngphnmdwjnkdhadsdc3x
    pubkey: cropub1addwnpepqt255g89h57ttkk2sdklsqn5pcc3z8fecrpe2rh3yka5xn484lx8k6gs8t6
    mnemonic: ""
    threshold: 0
    pubkeys: []


    **Important** write this mnemonic phrase in a safe place.
    It is the only way to recover your account if you ever forget your password.

    ## THIS IS YOUR mnemonic, MAKE SURE YOU HAVE SAVED IT ##

    ```

    The key comes with a "seed phrase", which is serialized into a human-readable 24-word mnemonic. User can recover their associated addresses with the mnemonic phrase. It is **important** that you keep the mnemonic for address secure, as there is **no way** to recover it. You would not be able to recover and access the funds in the wallet if you forget the mnemonic phrase.



    You should obtain an address with `cro` prefix, e.g. `cro1uayxmr8597lfg27q2zngphnmdwjnkdhadsdc3x`. This will be the address we would need you to provide for initial allocation of funds.


- List and check your key: We should be able to see the key `cross-fire-testing` while using the `keys list` command: 

    ```bash 
    $ ./chain-maind keys list
    - name: cross-fire-testing
    type: local
    address: cro1uayxmr8597lfg27q2zngphnmdwjnkdhadsdc3x
    pubkey: cropub1addwnpepqt255g89h57ttkk2sdklsqn5pcc3z8fecrpe2rh3yka5xn484lx8k6gs8t6
    mnemonic: ""
    threshold: 0
    pubkeys: []
    ```

    **Remarks** : If the key did not show up as expected, (i.e. It gives you an empty output `[]` ), kindly add the `--keyring-backend="file"` instead, for example: 
    ```bash 
    $ ./chain-maind keys add cross-fire-file-keyring --keyring-backend="file"
    ```
    and list it by
    ```bash
    $ ./chain-maind keys list --keyring-backend="file"
    ```

- If you have any question, kindly go to Crossfire channel (under #crossfire): [![Support Server](https://img.shields.io/discord/783264383978569728.svg?color=7289da&label=Crypto.com-Chain&logo=discord&style=flat-square)](https://discord.gg/pahqHz26q4) and seek for assistance. 




