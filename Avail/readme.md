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
### System
```
sudo tee /etc/systemd/system/avail.service > /dev/null <<EOF
[Unit]
Description=Avail Light Client
After=network.target
StartLimitIntervalSec=0

[Service]
User=$USER
RestartSec=10
ExecStart=$HOME/.avail/bin/avail-light --network goldberg --config $HOME/.avail/config/config.yml --app-id 0 --identity $HOME/.avail/identity/identity.toml
Restart=always
LimitNOFILE=65000

[Install]
WantedBy=multi-user.target
EOF
```
```
sudo systemctl daemon-reload
sudo systemctl enable availd
sudo systemctl start availd
```
```
journalctl -u availd -fo cat
```
