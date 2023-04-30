### Update 
```
sudo apt update && \
sudo apt upgrade
```

### Download Dockers
```
sudo apt-get update && sudo apt install jq && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin && sudo apt-get install docker-compose-plugin
apt install docker-compose
```
## Create Elixir Folder
```
mkdir elixir && cd elixir
```

### Binary
```
wget https://testnet-2-files.elixir.finance/Dockerfile
```

### Set config `Address Metamask & PK`
```
nano Dockerfile
```

### Run
```
docker build . -f Dockerfile -t elixir-validator
```

```
docker run -d --restart unless-stopped --name ev elixir-validator
```

### Cek log
```
docker logs -f ev
```

### Update Ver
```
docker kill ev
docker rm ev
docker pull elixirprotocol/validator:testnet-2
docker build . -t elixir-validator
docker run --restart unless-stopped -d --name ev elixir-validator
```



