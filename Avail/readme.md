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
rm -r avail-light-linux-amd64.tar.gz
```
### Go to directory
```
cd /avail-light/target/release
```
### Create identity.toml file:
```
touch identity.toml
nano identity.toml
```
### )Paste this and enter your avail wallet seed:
```
avail_secret_seed_phrase = 'enter_your_seed_here'
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
sudo systemctl daemon reload
sudo systemctl enable avail
sudo systemctl start avail
```
```
journalctl -u avail -fo cat
```
