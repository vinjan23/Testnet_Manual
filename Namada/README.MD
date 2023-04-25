### NAMADA TESTNET

### Update Tool
```
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc pkg-config libssl-dev libclang-dev -y
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source "$HOME/.cargo/env"
```

### Replace Validator Name & Wallet Name
```
echo "export ALIAS="CHOOSE_A_NAME_FOR_YOUR_VALIDATOR"" >> $HOME/.bash_profile
echo "export WALLET="CHOOSE_A_WALLET_NAME"" >> $HOME/.bash_profile
echo "export PUBLIC_IP=$(wget -qO- eth0.me)" >> $HOME/.bash_profile
echo "export TM_HASH="v0.1.4-abciplus"" >> $HOME/.bash_profile
echo "export CHAIN_ID="public-testnet-7.0.3c5a38dc983"" >> $HOME/.bash_profile
source $HOME/.bash_profile
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

### Run Node
```
cd $HOME
namada client utils join-network --chain-id $CHAIN_ID --genesis-validator $ALIAS
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

### Set Connection
```
sudo ufw default allow outgoing 
sudo ufw default deny incoming 
sudo ufw allow ssh/tcp 
sudo ufw allow 26656,26657/tcp
sudo ufw enable
```

### Create Wallet

```
namada wallet address gen --alias $WALLET
```
```
namadac transfer \
    --token NAM \
    --amount 1000 \
    --source faucet \
    --target $ALIAS \
    --signer $ALIAS
```
    
### Sync
```
curl http://127.0.0.1:26657/status | jq .result.sync_info.catching_up
```

### Check Balances
```
namada client balance --owner $ALIAS --token NAM
```

### Init Validator
```
namada client init-validator \
  --alias $ALIAS \
  --source $WALLET \
  --commission-rate 0.05 \
  --max-commission-rate-change 0.01
```

### Stake Funds
```
namada client bond \
  --validator $ALIAS \
  --amount 1500 \
  --gas-limit 10000000
```

### Check Status
```
namada client bonded-stake
```

  
atest1v4ehgw36xyurgdfjgcurgdzrggc52s3sxaprwv6rgge5x3fnxc65xd2yx4rygd2rx3q5xvp3xc66pw







