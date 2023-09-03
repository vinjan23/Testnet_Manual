### Package
```
sudo apt update && sudo apt install curl -y
```
```
sudo apt update && sudo apt install ocl-icd-opencl-dev libopencl-clang-dev libgomp1 -y
```
### Vars
```
NODENAME=<YOUR_NODENAME>
WALLET_ADDRESS=<YOUR_WALLET_ADDRESS>
PLOT_SIZE=100G
```
### Import
```
echo "export NODENAME=$NODENAME" >> $HOME/.bash_profile
echo "export WALLET_ADDRESS=$WALLET_ADDRESS" >> $HOME/.bash_profile
echo "export PLOT_SIZE=$PLOT_SIZE" >> $HOME/.bash_profile
source $HOME/.bash_profile
```
### Binary
```
cd $HOME
rm -rf subspace-*
wget -O subspace-node https://github.com/subspace/subspace/releases/download/gemini-3f-2023-aug-31/subspace-node-ubuntu-x86_64-skylake-gemini-3f-2023-aug-31
wget -O subspace-farmer https://github.com/subspace/subspace/releases/download/gemini-3f-2023-aug-31/subspace-farmer-ubuntu-x86_64-v2-gemini-3f-2023-aug-31
chmod +x subspace-*
mv subspace-* /usr/local/bin/
```
### Service Subspace Node
 ```
tee $HOME/subspaced.service > /dev/null <<EOF
[Unit]
Description=Subspace Node
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which subspace-node) --chain gemini-3f --execution wasm --state-pruning archive --validator --name $NODENAME
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
mv $HOME/subspaced.service /etc/systemd/system/
```
### Service Subspace Farmer
```
tee $HOME/subspaced-farmer.service > /dev/null <<EOF
[Unit]
Description=Subspaced Farm
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which subspace-farmer) farm --reward-address $WALLET_ADDRESS --plot-size $PLOT_SIZE
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
mv $HOME/subspaced-farmer.service /etc/systemd/system/
```
### Start
```
sudo systemctl restart systemd-journald
sudo systemctl daemon-reload
sudo systemctl enable subspaced subspaced-farmer
sudo systemctl restart subspaced
sudo systemctl restart subspaced-farmer
```
### Cek Node status
```
service subspaced status
```
### Cek Logs
```
journalctl -u subspaced -f -o cat
```
### Cek Farmer status
```
service subspaced-farmer status
```
### Cek Logs
```
journalctl -u subspaced-farmer -f -o cat
```

### Delete Node
```
sudo systemctl stop subspaced subspaced-farmer
sudo systemctl disable subspaced subspaced-farmer
rm -rf ~/.local/share/subspace*
rm -rf /etc/systemd/system/subspaced*
rm -rf /usr/local/bin/subspace*
```


