<p align="center">
 <img height="200" height="auto" src="https://user-images.githubusercontent.com/34649601/206725652-b4621198-ab00-4943-8cba-b0f47c5a2502.png">


# Official Links
### [Official Document](https://docs.qtestnet.org/how-to-setup-validator/)

# Explorer
### [EXPLORER](https://stats.qtestnet.org/)

### Set vars
```
PASSWORD=YOUR_PASSWORD
```
Change `YOUR_PASSWORD` to secure password

Export the variable 
```
echo "export PASSWORD=$PASSWORD" $HOME/.bash_profile
source $HOME/.bash_profile
```

### Update
```
sudo apt update && \
sudo apt upgrade
```

### Install Docker
```
sudo apt-get update && sudo apt install jq && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin && sudo apt-get install docker-compose-plugin
apt install docker-compose
```

### Download Binary and set Password file
```
git clone https://gitlab.com/q-dev/testnet-public-tools.git
cd testnet-public-tools/testnet-validator/
```
```
mkdir keystore
cd keystore/
echo "$PASSWORD" >> pwd.txt
```

### Generate Wallet
```
cd ..
docker run --entrypoint="" --rm -v $PWD:/data -it qblockchain/q-client:testnet geth account new --datadir=/data --password=/data/keystore/pwd.txt
```
SAVE THE OUTPUT!!!

### Fund your Wallet
Fund your wallet [ Q Faucet ](https://faucet.qtestnet.org/)

### Configure
```
cp .env.example .env
nano .env
```

Add Address without `0x` and your Public IP and save settings `CTRL+X` `y` and `ENTER`
 
 Add IP `Your IP`
 
Change to `QCLIENT_IMAGE=qblockchain/q-client:1.2.2`

Example : 

 <img height="400" height="auto" src="https://user-images.githubusercontent.com/34649601/206742445-4c80d903-09bd-4e80-ac1f-f8711ec4f8f2.png">

Configure `config.json`
```
nano config.json
```
Add Address without `0x` and your Password from the beginning and save `CTRL+X` `y` and `ENTER`

<img height="400" height="auto" src="https://user-images.githubusercontent.com/34649601/206742620-1c42d1a7-34c6-4b8c-a1da-95b2d0e3b577.png">

### Stake to the contract
```
docker run --rm -v $PWD:/data -v $PWD/config.json:/build/config.json qblockchain/js-interface:testnet validators.js
```

### Export your private key to Metamask 
```
cd testnet-public-tools
chmod +x run-js-tools-in-docker.sh
./run-js-tools-in-docker.sh
npm install
```
```
chmod +x extract-geth-private-key.js
node extract-geth-private-key <WALLET_ADDRESS> ../testnet-validator/ $PASSWORD
```
Change `<WALLET_ADDRESS>` To your Wallet address

After saving your private key exit the container `exit`

# Registering Validator

In order to register your node you have to register in the Form : [Register Form](https://itn.qdev.li/)

Register your validator according to your validator info

After successfully register your validator you will receive your validator name 

<img height="400" height="auto" src="https://user-images.githubusercontent.com/34649601/206744494-8418897e-226c-49be-a073-4ed939084384.png">

### Configure docker-compose.yaml
```
cd testnet-validator
nano docker-compose.yaml
```
Then add `--ethstats=ITN-testname-ecd07:qstats-testnet@stats.qtestnet.org` 

Example : 

<img height="450" height="auto" src="https://user-images.githubusercontent.com/34649601/206747640-e29e7f73-a549-416a-b52f-6a138f402212.png">

Then Run the Validator : 
```
docker compose up -d
```

Check logs ( You have to be in the compose directory! )
```
docker compose logs -f
```
CTRL+C to exit logs

## Check your validator 

### [Q Explorer](https://stats.qtestnet.org/)

### Hapus Node
```
sudo docker-compose down -v
rm -rf testnet-public-tools
```

# Move to New VPS

### Stop Old VPS
```
cd testnet-public-tools/testnet-validator/
docker-compose down -v
```

Change this 3 Value 
`Download Folder testnet-validator`

# Install Docker
 ```
sudo apt update
sudo apt upgrade
apt install docker-compose
sudo apt-get update && sudo apt install jq && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin && sudo apt-get install docker-compose-plugin
```
# Install Node
```
git clone https://gitlab.com/q-dev/testnet-public-tools
cd testnet-public-tools
rm -fr testnet-validator
cd
```
# Upload Folder testnet-validator into testnet-public-tools

# Open your .env file and replace the ip address of your new server with the old one
```
cd testnet-public-tools/testnet-validator/
nano .env 
```
# Wallet Creation with PK
```
apt install npm
npm install ethereumjs-wallet
```
# Create file export-key-as-json.js with the following content; Upload this file to the main directory
```
const fs = require("fs")
const wallet = require("ethereumjs-wallet").default

const pk = new Buffer.from(process.argv[2], 'hex') // replace by correct private key
const account = wallet.fromPrivateKey(pk)
const password = process.argv[3] // will be required to unlock/sign after importing to a wallet like MyEtherWallet

account.toV3(password)
    .then(value => {
        const address = account.getAddress().toString('hex')
        const file = `UTC--${new Date().toISOString().replace(/[:]/g, '-')}--${address}.json`
        fs.writeFileSync(file, JSON.stringify(value))
    });
```
# `Run node export-key-as-json.js <your-private-key> <some-random-password>`

# You can now upload this file you created into the keystore folder

### Run your docker-compose
```
docker compose up -d
docker compose logs -f
```
To check logs You have to be in the compose directory!

`CTRL+C` to exit logs










