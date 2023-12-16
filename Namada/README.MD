### NAMADA TESTNET

### Update Tool
```
sudo apt update && sudo apt upgrade -y && \
sudo apt install curl build-essential git wget jq make gcc tmux htop nvme-cli pkg-config libssl-dev tar clang bsdmainutils ncdu protobuf-compiler unzip libudev-dev libleveldb-dev -y
```
### Install GO
```
cd $HOME && \
ver="1.20.5" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```
### Install Rust
```
sudo curl https://sh.rustup.rs -sSf | sh -s -- -y
source "$HOME/.cargo/env"
cargo --version
```
### Install Protoc
```
cd $HOME && rustup update  
PROTOC_ZIP=protoc-23.3-linux-x86_64.zip
curl -OL https://github.com/protocolbuffers/protobuf/releases/download/v23.3/$PROTOC_ZIP 
sudo unzip -o $PROTOC_ZIP -d /usr/local bin/protoc 
sudo unzip -o $PROTOC_ZIP -d /usr/local 'include/*' 
rm -f $PROTOC_ZIP 
protoc --version
```
### Change 
```
echo "export VALIDATOR_ALIAS=YOUR_MONIKER" >> ~/.bash_profile
echo "export EMAIL=your_validator_email" >> ~/.bash_profile
```
### Set Vars
```
echo "export NAMADA_TAG=v0.28.0" >> ~/.bash_profile
echo "export CBFT=v0.37.2" >> ~/.bash_profile
echo "export NAMADA_CHAIN_ID=public-testnet-15.0dacadb8d663" >> ~/.bash_profile
echo "export KEY_ALIAS=wallet" >> ~/.bash_profile
echo "export BASE_DIR=$HOME/.local/share/namada" >> ~/.bash_profil
```

### Namada Binary
```
git clone https://github.com/anoma/namada && cd namada && git fetch && git checkout $NAMADA_TAG 
make build-release
```
```
COMETBFT_TAG="v0.37.2"
cd $HOME && git clone https://github.com/cometbft/cometbft.git && cd cometbft && git checkout $CBFT
make build
```
### move
```
cd $HOME && cp $HOME/cometbft/build/cometbft /usr/local/bin/cometbft && \
cp "$HOME/namada/target/release/namada" /usr/local/bin/namada && \
cp "$HOME/namada/target/release/namadac" /usr/local/bin/namadac && \
cp "$HOME/namada/target/release/namadan" /usr/local/bin/namadan && \
cp "$HOME/namada/target/release/namadaw" /usr/local/bin/namadaw && \
cp "$HOME/namada/target/release/namadar" /usr/local/bin/namadar
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
WorkingDirectory=$HOME/.local/share/namada
Environment=TM_LOG_LEVEL=p2p:none,pex:error
Environment=NAMADA_CMT_STDOUT=true
ExecStart=/usr/local/bin/namada node ledger run 
StandardOutput=syslog
StandardError=syslog
Restart=always
RestartSec=10
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### start

```
sudo systemctl daemon-reload
sudo systemctl enable namadad
```
### Post Genesis
```
cd $HOME && namada client utils join-network --chain-id $NAMADA_CHAIN_ID
```
```
sudo systemctl restart namadad && sudo journalctl -u namadad -f -o cat
```

### Create Wallet
```
namada wallet key gen --alias $KEY_ALIAS --unsafe-dont-encrypt
namada wallet address find --alias $KEY_ALIAS
```

### Check Balances
```
namada client balance --owner $KEY_ALIAS --token NAM
```
### Validator
```
cd $HOME
namada client init-validator \
--alias $VALIDATOR_ALIAS \
--account-keys $KEY_ALIAS \
--signing-keys $KEY_ALIAS \
--commission-rate 0.05 \
--max-commission-rate-change 0.01 \
--email $EMAIL \
--unsafe-dont-encrypt
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








