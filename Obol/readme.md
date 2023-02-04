## Update
```
sudo apt update && sudo apt upgrade
sudo apt update
sudo apt install git
```

## Docker
```
sudo apt-get update && sudo apt install jq && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin && sudo apt-get install docker-compose-plugin
apt install docker-compose
```

## Build
```
git clone https://github.com/ObolNetwork/charon-distributed-validator-node.git
```
```
mkdir .charon && sudo chmod 777 .charon
```

##
```
cd charon-distributed-validator-node
docker run --rm -v "$(pwd):/opt/charon" obolnetwork/charon:v0.13.0 create enr
```
