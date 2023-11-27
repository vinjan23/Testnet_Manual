
### Clean up data and binaries from any previous testnets
```
rm -rf ~/libra-framework && rm -rf ~/.libra/libra-legacy-v6
rm -rf ~/.libra/data && rm -rf ~/.libra/genesis && rm -rf ~/.libra/secure-data.json
rm -f /usr/bin/libra && rm -rf /usr/local/bin/libra && rm -f ~/.cargo/bin/libra
```

### Build

```
cd $HOME
git clone https://github.com/0LNetworkCommunity/libra-framework
cd libra-framework
git fetch --all
git checkout release-6.9.0-rc.10
```
```
git log -n 1 --pretty
```

```
sudo apt install make
```

### Make Libra-framework folder
```
mkdir ~/.libra
```
### Change directory to the genesis tool
```
cd libra-framework/tools/genesis
```

### Install source and reload bash
```
make install
```
 ```
source ~/.bashrc
```
### Setup the validator configs and data directory

### Make wallet
```
libra wallet keygen
```
### Init
```
libra config validator-init
```

### Retrieve Validator address
```
grep 'account_address' ~/.libra/public-keys.yaml
```

### Paste your GitHub PAT (Github Token Classis)
```
nano ~/.libra/github_token.txt
```
`Crtl+X+Y Enter`

```
export GIT_REPO=release-v6.9.0-genesis-registration
```
```
make register
```
```
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
```
