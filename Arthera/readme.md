### Update
```
sudo apt update && sudo apt upgrade
sudo apt update
sudo apt install git
```
### Docker
```
sudo apt-get update && sudo apt install jq && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin && sudo apt-get install docker-compose-plugin
apt install docker-compose
```
### Build
```
docker pull arthera/arthera-node:latest
```
```
mkdir $HOME/arthera
```
### Create Wallet
```
docker run -it -v $HOME/arthera:/data arthera/arthera-node:latest account new
```
### Create Validator Identity
```
docker run -it -v $HOME/arthera:/data arthera/arthera-node:latest validator new
```
### Reset
```
rm -rf $HOME/arthera/arthera-node $HOME/arthera/chaindata
```
```
rm -rf $HOME/arthera/devnet.g
```
```
curl -o $HOME/arthera/devnet.g http://release.arthera.net/devnet3.g
```

### Stop
```
docker ps
```
```
docker stop <>
```
```
docker rm <>
```
```
docker image list
```
```
docker rmi <>
```
```
docker stop arthera
docker run --rm -it -v $HOME/arthera:/data arthera/arthera-node:latest db heal --experimental
docker restart arthera
```
