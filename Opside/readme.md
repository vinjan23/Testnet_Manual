### Hardware requirements

The following requirements are recommended

- 4 or more physical CPU cores
- At least 200GB of SSD disk storage
- At least 16GB of memory (RAM)
- At least 100mbps network bandwidth

### Install Auto
```
wget -c https://pre-alpha-download.opside.network/testnet-auto-install-v2.tar.gz 
tar -C ./ -xzf testnet-auto-install-v2.tar.gz
chmod +x -R ./testnet-auto-install-v2
cd ./testnet-auto-install-v2
```

### Install validator clients
```
./install-ubuntu-en-1.0.sh
```

Follow the CLI prompts to generate your keys. You will need to enter:
1. Your withdrawal Opside address(which is used to receive your validator rewards and your deposit when you exit)
2. Password(which is used to encrypt your validator signing key)
3. Repeat your withdrawal Opside address
4. Repeat your Password
Then there will be 24 mnemonic seed phrases. This is highly sensitive and should never be exposed to other people or networked hardware.
. You should now have your mnemonic written down in a safe place and a keystore saved for each of your validators.
. Please make sure you keep these safe, preferably offline.

Follow the CLI prompts to:
. Enter your seed phrase(separated with space)
. Waiting for the validator key generated
. Waiting for the nodes launched


### Check the logs
```
cd ./testnet-auto-install-v2/testnet-auto-install-v3
```
```
opside-chain/show-geth-log.sh
```

```
show the consensus client logs
```
```
opside-chain/show-beaconChain-log.sh
```
```
opside-chain/show-validator-log.sh
```

### Deposit
### Upload Deposit file
### Go to [Validator Launchpad](https://opside.network/validator/deposit)
Follow the steps to enter the Upload deposit data page, then upload the deposit data file you just generated.The deposit_data-[timestamp].json is located in directory testnet-auto-install/validator_keys/.

### Restart
```
cd ./testnet-auto-install-v2/testnet-auto-install-v3
```

```
stop-all.sh
```
```
opside-chain/start-geth.sh
```
```
opside-chain/start-beaconChain.sh
```
```
opside-chain/start-validator.sh
```
