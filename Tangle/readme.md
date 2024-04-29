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
wget -O tangle https://github.com/webb-tools/tangle/releases/download/v1.0.0/tangle-default-linux-amd64 && chmod +x tangle
mv tangle /usr/bin/
```

```
tangle --version
```

### Create System `<change moniker>`
```
tee /etc/systemd/system/tangle.service > /dev/null << EOF
[Unit]
Description=Tangle Validator Node
After=network-online.target
StartLimitIntervalSec=0
[Service]
User=$USER
Restart=always
RestartSec=3
LimitNOFILE=65535
ExecStart=/usr/bin/tangle \
  --base-path $HOME/.tangle/data/ \
  --name '$yourname' \
  --chain tangle-mainnet \
  --node-key-file "$HOME/.tangle/node-key" \
  --port 30333 \
  --rpc-port 9933 \
  --prometheus-port 9615 \
  --pruning archive \
  --validator \
  --auto-insert-keys \
  --telemetry-url "wss://telemetry.polkadot.io/submit 0" \
  --no-mdns
[Install]
WantedBy=multi-user.target
EOF
```
```
systemctl daemon-reload
systemctl enable tangle
```
```
tangle key insert \
--scheme Sr25519 \
--suri "kelimeleri yaz" \
--base-path $HOME/.tangle/data/ \
--chain tangle-testnet \
--key-type acco
```
```
tangle key insert --base-path $HOME/.tangle/data/ \
--chain $HOME/.tangle/tangle-mainnet.json \
--scheme Sr25519 \
--suri "beach then gift govern begin fork cloud actor finger subject title idea arrow area tonight service employ powder open solid lend surface maid legend" \
--key-type babe
```
```
tangle key insert --base-path $HOME/.tangle/data/ \
--chain $HOME/.tangle/tangle-mainnet.json \
--scheme Ed25519 \
--suri "<beach then gift govern begin fork cloud actor finger subject title idea arrow area tonight service employ powder open solid lend surface maid legend>" \
--key-type gran
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


