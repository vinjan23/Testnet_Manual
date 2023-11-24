### Update
```
apt update && apt upgrade -y
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev libgmp3-dev tar clang bsdmainutils ncdu unzip llvm libudev-dev make protobuf-compiler -y
```

### Binary
```
mkdir $HOME/.tangle
```
```
cd $HOME/.tangle
wget -O tangle https://github.com/webb-tools/tangle/releases/download/v5.0.0/tangle-standalone-linux-amd64
chmod 744 tangle
mv tangle /usr/bin/
```

### json
```
wget -O $HOME/.tangle/tangle-standalone.json "https://raw.githubusercontent.com/webb-tools/tangle/main/chainspecs/testnet/tangle-standalone.json"
chmod 744 ~/.tangle/tangle-standalone.json
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
  --name $MONIKER \
  --chain $HOME/.tangle/tangle-standalone.json \
  --node-key-file "$HOME/.tangle/node-key" \
  --port 30333 \
  --rpc-port 9933 \
  --prometheus-port 9615 \
  --auto-insert-keys \
  --validator \
  --telemetry-url "wss://telemetry.polkadot.io/submit 0" \
  --no-mdns
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


