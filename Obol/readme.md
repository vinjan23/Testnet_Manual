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
cd charon-distributed-validator-node
```
```
mkdir .charon && sudo chmod 777 .charon
```

## Build ENR Key
```
docker run --rm -v "$(pwd):/opt/charon" obolnetwork/charon:v0.13.0 create enr
```
`output like this`
`Created ENR private key: .charon/charon-enr-private-key
enr:-JG4QGQpV4qYe32QFUAbY1UyGNtNcrVMip83cvJRhw1brMslPeyELIz3q6dsZ7GblVaCjL_8FKQhF6Syg-O_kIWztimGAYHY5EvPgmlkgnY0gmlwhH8AAAGJc2VjcDI1NmsxoQKzMe_GFPpSqtnYl-mJr8uZAUtmkqccsAx7ojGmFy-FY4N0Y3CCDhqDdWRwgg4u`

## Run
```
docker-compose up
```
## Logs All
```
cd charon-distributed-validator-node
docker compose logs -f
```

## Logs charon
```
cd charon-distributed-validator-node
docker compose logs -f charon
```

## Logs Geth
```
cd charon-distributed-validator-node
docker compose logs -f geth
```
## Logs Grafana
```
cd charon-distributed-validator-node
docker compose logs -f grafana
```

## Logs Lighthouse
```
cd charon-distributed-validator-node
docker compose logs -f lighthouse
```
## Logs Teku
```
cd charon-distributed-validator-node
docker compose logs -f teku
```

## Logs prometheus
```
cd charon-distributed-validator-node
docker compose logs -f prometheus
```
