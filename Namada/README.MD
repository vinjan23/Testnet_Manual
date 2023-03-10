### NAMADA TESTNET

### Update Tool
```
cd $HOME
sudo apt update && sudo apt upgrade -y
sudo apt install curl tar wget clang pkg-config libssl-dev libclang-dev jq build-essential bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
sudo apt install -y uidmap dbus-user-session
```

### Setup RUST
```
cd $HOME
  sudo apt update
  sudo curl https://sh.rustup.rs -sSf | sh -s -- -y
  . $HOME/.cargo/env
  curl https://deb.nodesource.com/setup_16.x | sudo bash
  sudo apt install cargo nodejs -y < "/dev/null"
  ```

### Install GO

```
  ver="1.19.4"
  cd $HOME
  wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
  sudo rm -rf /usr/local/go
  sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
  rm "go$ver.linux-amd64.tar.gz"
  echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
  source ~/.bash_profile
```

### Setup Var

```
echo "export VALIDATOR_ALIAS=change_your_validator_name" >> ~/.bash_profile
echo "export WALLET=change_your_wallet_name" >> ~/.bash_profile
echo "export NAMADA_TAG=v0.12.1" >> ~/.bash_profile
echo "export TM_HASH=v0.1.4-abciplus" >> ~/.bash_profile
echo "export CHAIN_ID=public-testnet-1.0.05ab4adb9db" >> ~/.bash_profile
source ~/.bash_profile
```

### Build Binary

```
cd $HOME && git clone https://github.com/anoma/namada && cd namada && git checkout $NAMADA_TAG
make build-release
cargo --version
```

### Build Tendermint

```
cd $HOME && git clone https://github.com/heliaxdev/tendermint && cd tendermint && git checkout $TM_HASH
make build
cd $HOME && cp $HOME/tendermint/build/tendermint  /usr/local/bin/tendermint && cp "$HOME/namada/target/release/namada" /usr/local/bin/namada && cp "$HOME/namada/target/release/namadac" /usr/local/bin/namadac && cp "$HOME/namada/target/release/namadan" /usr/local/bin/namadan && cp "$HOME/namada/target/release/namadaw" /usr/local/bin/namadaw
tendermint version
```

### Setup Full Node
```
cd $HOME && namada client utils join-network --chain-id $CHAIN_ID
cd $HOME && wget https://github.com/heliaxdev/anoma-network-config/releases/download/public-testnet-1.0.05ab4adb9db/public-testnet-1.0.05ab4adb9db.tar.gz
tar xvzf "$HOME/public-testnet-1.0.05ab4adb9db.tar.gz"
```

### Create Service File

```
sudo tee /etc/systemd/system/namadad.service > /dev/null <<EOF
[Unit]
Description=namada
After=network-online.target

[Service]
User=root
WorkingDirectory=$HOME/.namada
Environment=NAMADA_LOG=debug
Environment=NAMADA_TM_STDOUT=true
ExecStart=/usr/local/bin/namada --base-dir=$HOME/.namada node ledger run 
StandardOutput=syslog
StandardError=syslog
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
cd $HOME
namada wallet address gen --alias $WALLET
namadac transfer \
    --token NAM \
    --amount 1000 \
    --source faucet \
    --target $WALLET \
    --signer $WALLET
```

### Create Client

```
namada client init-validator --alias $VALIDATOR_ALIAS --source $WALLET --commission-rate 0.05 --max-commission-rate-change 0.01 --gas-limit 10000000
```







