"# delete any forked release-v6.9.0-rc.0-genesis-X in your GitHub org  

# Clean up data and binaries from any previous testnets
rm -rf ~/libra-framework && rm -rf ~/.libra/libra-legacy-v6
rm -rf ~/.libra/data && rm -rf ~/.libra/genesis && rm -rf ~/.libra/secure-data.json
rm -f /usr/bin/libra && rm -rf /usr/local/bin/libra && rm -f ~/.cargo/bin/libra"  "cd ~
git clone https://github.com/0LNetworkCommunity/libra-framework
cd ~/libra-framework
git fetch --all && git checkout release-6.9.0-rc.10

git log -n 1 --pretty=format:""%H"""  "#Install make
sudo apt install make

# export SOURCE_PATH=... as needed

# Export the epoch from which legacy is built
export EPOCH=692

# Make .libra folder
mkdir ~/.libra

# Change directory to the genesis tool
cd ~/libra-framework/tools/genesis

# Install source and reload bash
make install && source ~/.bashrc"  "# Setup the validator configs and data directory
libra config validator-init

# Retrieve Validator address and paste it aside
grep 'account_address' ~/.libra/public-keys.yaml

# Paste your GitHub PAT in the
nano ~/.libra/github_token.txt"  "export GIT_REPO=release-v6.9.0-genesis-registration

make register"

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

