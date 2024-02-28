### Update
```
sudo apt update && sudo apt upgrade -y
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

### Binary
```
cd $HOME
mkdir -p $HOME/.tangle
cd $HOME/.tangle
wget -O tangle https://github.com/webb-tools/tangle/releases/download/v0.6.1/tangle-testnet-linux-amd64 && chmod +x tangle
mv tangle /usr/bin/
```
```
tangle --version
```

### Create System `<change moniker>`
```
sudo tee /etc/systemd/system/tangle.service > /dev/null << EOF
[Unit]
Description=Tangle Validator Node
After=network-online.target
StartLimitIntervalSec=0
[Service]
User=$USER
Restart=always
RestartSec=3
ExecStart=/usr/bin/tangle \
  --base-path $HOME/.tangle/data/validator/$yourname \
  --name '$MONIKER' \
  --chain tangle-testnet \
  --node-key-file "/home/$yourname/node-key" \
  --port 30333 \
  --rpc-port 9933 \
  --prometheus-port 9515 \
  --telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
  --validator \
  --no-mdns
  ----pruning archive
[Install]
WantedBy=multi-user.target
EOF
```

### Start
```
systemctl daemon-reload
systemctl enable tangle
systemctl restart tangle
journalctl -u tangle -f -o cat
```

- Check :
> [Polkadot Telemetry](https://telemetry.polkadot.io/#list/0xea63e6ac7da8699520af7fb540470d63e48eccb33f7273d2e21a935685bf1320")

### Setup Keys
```
curl -H "Content-Type: application/json" -d '{ "id": 1, "jsonrpc": "2.0", "method": "author_rotateKeys", "params": [] }' http://localhost:9933
```

### Input Keys
> [Polkadot Substrate](https://polkadot.js.org/apps/#/accounts")


