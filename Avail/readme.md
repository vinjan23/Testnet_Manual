### Create Directory
```
cd $HOME
mkdir -p avail-light-client
cd avail-light-client
```

### Download Binary
```
wget https://github.com/availproject/avail-light/releases/download/v1.7.10/avail-light-linux-amd64.tar.gz
tar -xvzf avail-light-linux-amd64.tar.gz
cp avail-light-linux-amd64 avail-light
```
```
sudo su
```
```
curl -sL1 avail.sh | bash
```
### System
```
sudo tee /etc/systemd/system/availd.service > /dev/null <<EOF
[Unit]
Description=Avail Light Client
After=network-online.target

[Service]
User=$USER
RestartSec=3
ExecStart=$HOME/.avail/bin/avail-light --network goldberg --config $HOME/.avail/config/config.yml --app-id 0 --identity $HOME/.avail/identity/identity.toml
Restart=always
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```
```
sudo systemctl daemon-reload
sudo systemctl enable availd
```
```
sudo systemctl restart availd
```
```
journalctl -u availd -fo cat
```
### Latest Block
```
curl "http://127.0.0.1:7000/v1/latest_block"
```
