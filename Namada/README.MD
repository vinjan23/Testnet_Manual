### NAMADA TESTNET

### Update Tool
```
sudo apt update && sudo apt upgrade -y
sudo apt install libclang-12-dev
```
### Install GO
```
cd $HOME
if ! [ -x "$(command -v go)" ]; then
wget -O go1.19.4.linux-amd64.tar.gz https://golang.org/dl/go1.19.4.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.19.4.linux-amd64.tar.gz && sudo rm go1.19.4.linux-amd64.tar.gz
echo 'export GOROOT=/usr/local/go' >> $HOME/.bash_profile
echo 'export GOPATH=$HOME/go' >> $HOME/.bash_profile
echo 'export GO111MODULE=on' >> $HOME/.bash_profile
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile && . $HOME/.bash_profile
go version
fi 
```
### Install Rust
```
cd $HOME && rustup update
PROTOC_ZIP=protoc-3.14.0-linux-x86_64.zip
curl -OL https://github.com/protocolbuffers/protobuf/releases/download/v3.14.0/$PROTOC_ZIP
sudo unzip -o $PROTOC_ZIP -d /usr/local bin/protoc
sudo unzip -o $PROTOC_ZIP -d /usr/local 'include/*'
rm -f $PROTOC_ZIP
```

### Replace Validator Name & Wallet Name
```
echo "export VALIDATOR_ALIAS=YOUR_MONIKER" >> ~/.bash_profile
echo "export NAMADA_TAG=v0.15.3" >> ~/.bash_profile
echo "export TM_HASH=v0.1.4-abciplus" >> ~/.bash_profile
echo "export CHAIN_ID=public-testnet-8.0.b92ef72b820" >> ~/.bash_profile
echo "export WALLET=wallet" >> ~/.bash_profile
echo "export BASE_DIR=$HOME/.local/share/namada" >> ~/.bash_profile
source ~/.bash_profile
```
### Make Dir
```
mkdir $HOME/.local/
mkdir $HOME/.local/share/
mkdir $HOME/.local/share/namada
```

### Namada Binary

```
cd $HOME
rm -rf namada
git clone https://github.com/anoma/namada
cd namada
git checkout v0.15.1
make build-release
sudo cp -r $HOME/namada/target/release/namada* /usr/local/bin/
```
```
namada --version
```

### Build Tendermint

```
cd $HOME
git clone https://github.com/heliaxdev/tendermint
cd tendermint
git checkout $TM_HASH
make build
sudo cp $HOME/tendermint/build/tendermint  /usr/local/bin/
```
```
tendermint version
#0.1.4-abciplus
```

### Run Node Pre-Genesis
```
cd $HOME
namada client utils join-network --chain-id $CHAIN_ID --genesis-validator $ALIAS
```
### Post-Genesis
```
namada client utils join-network --chain-id $CHAIN_ID
```

### Create Service File

```
sudo tee /etc/systemd/system/namadad.service > /dev/null <<EOF
[Unit]
Description=namada
After=network-online.target

[Service]
User=$USER
WorkingDirectory=$HOME
Environment=NAMADA_LOG=debug
Environment=NAMADA_TM_STDOUT=true
Environment="PATH=/bin:/usr/bin:/usr/local/bin"
ExecStart=/usr/local/bin/namada --base-dir=$HOME/.namada node ledger run
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### start

```
sudo systemctl daemon-reload
sudo systemctl enable namadad
sudo systemctl restart namadad && sudo journalctl -u namadad -f -o cat
```

### Create Wallet
```
namada wallet address gen --alias $WALLET
```
```
namada client transfer \
  --source faucet \
  --target $WALLET \
  --token NAM \
  --amount 1000 \
  --signer $WALLET
```
    
### Sync
```
curl http://127.0.0.1:26657/status | jq .result.sync_info.catching_up
```
### Check Address
```
curl -s localhost:26657/status | jq .result.validator_info.address
```
### Check Voting Power
```
curl -s localhost:26657/status | jq .result.validator_info.voting_power
```

### Check Balances
```
namada client balance --owner $ALIAS --token NAM
```

### Faucet
```
cd $HOME
namada client transfer \
    --token NAM \
    --amount 1000 \
    --source faucet \
    --target $VALIDATOR_ALIAS \
    --signer $VALIDATOR_ALIAS
```
```
namada client transfer \
    --token NAM \
    --amount 1000 \
    --source faucet \
    --target $VALIDATOR_ALIAS \
    --signer $VALIDATOR_ALIAS
```

### Init Validator
```
cd $HOME
namada client init-validator \
--alias $VALIDATOR_ALIAS \
--source $WALLET \
--commission-rate 0.05 \
--max-commission-rate-change 0.01 \
--signer $WALLET \
--gas-amount 100000000 \
--gas-token NAM \
--scheme ed25519 \
--unsafe-dont-encrypt
```

### Stake Funds
```
namada client bond \
--validator $VALIDATOR_ALIAS \
--amount 2000 \
--signer $VALIDATOR_ALIAS \
--source $VALIDATOR_ALIAS
```

### Check Status
```
namada client bonded-stake --validator $VALIDATOR_ALIAS
```
```
namada client bonds --validator $VALIDATOR_ALIAS
```

### Delete Node
```
systemctl stop namadad && systemctl disable namadad
rm /etc/systemd/system/namadad* -rf
rm $(which namadad) -rf
rm $HOME/.namada* -rf
rm $HOME/namada -rf
rm $HOME/tendermint -rf
```

atest1v4ehgw36xyurgdfjgcurgdzrggc52s3sxaprwv6rgge5x3fnxc65xd2yx4rygd2rx3q5xvp3xc66pw







