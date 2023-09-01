### Package
```
sudo apt update && sudo apt list --upgradable && sudo apt upgrade -y
```
```
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu net-tools -y
```
### GO
```
ver="1.20.4"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
source ~/.bash_profile
go version
```
### Binary
```
mkdir -p .erbie/erbie
cd $HOME
git clone https://github.com/erbieio/erbie
cd erbie
git checkout v0.14.5
go build -o erbie cmd/erbie/main.go
mv erbie /usr/local/bin
```
```
erbie version
```
### Service
```
sudo tee /etc/systemd/system/erbied.service > /dev/null <<EOF
[Unit]
Description=erbie
After=online.target
[Service]
Type=simple
User=$USER
WorkingDirectory=$HOME
ExecStart= /usr/local/bin/erbie \
  --datadir $HOME/.erbie \
  --devnet \
  --identity vinjan \
  --mine \
  --miner.threads 1 \
  --rpc \
  --rpccorsdomain "*" \
  --rpcvhosts "*" \
  --http \
  --rpcaddr 127.0.0.1 \
  --rpcport 23545 \
  --port 30303 \
  --maxpeers 50 \
  --syncmode full
Restart=on-failure
RestartSec=5
LimitNOFILE=4096
[Install]
WantedBy=multi-user.target
EOF
```
### Edit PK
```
nano .erbie/erbie/nodekey
```
### Start
```
sudo systemctl daemon-reload
sudo systemctl enable erbied
sudo systemctl start erbied
```
### Log
```
journalctl -fu erbied -o cat
```
### Instal Monitor
```
wget -O monitor.sh https://raw.githubusercontent.com/vinjan23/Testnet_Manual/main/erbie/monitor.sh && chmod +x monitor.sh && ./monitor.sh
```
```
./monitor.sh
```
### Delete
```
systemctl stop erbied
systemctl disable erbied
rm -rf root/usr/local/bin/erbie
rm -rf erbie
rm -rf erbie.sh
rm -rf monitor.sh
rm -rf .erbie
```
