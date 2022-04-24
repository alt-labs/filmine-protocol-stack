# filmine-protocol-stack

## Run custom 512MiB custom Filecoin network

Requirements are standard for Filecoin's lotus daemon:


```
git clone https://github.com/alt-labs/lotus
cd lotus
git checkout altnet
make clean deps altnet
sudo make install
```

Using these binaries folow the guide to run a custom network:
https://gist.github.com/ribasushi/291d4c9ec3f636f097b23a46d5fc3bce/13bc20684e87019dae8fdd7fb8ea5bcbfe53946b

## Run database
```
docker-compose up -d timescaledb

```

## Setup lily
```
git clone
cd lily
make clean butterflynet
sudo cp lily /usr/local/bin
```

Follow the setup from: https://lilium.sh/software/lily/setup/

Which basically sums up to
```
lotus chain export chain.car
lily init  --import-snapshot=chain.car
lily migrate --db='postgres://postgres:hakabdoanoij312332123123maaamamaa@localhost:5435/lily?sslmode=disable' --latest --name lily
```

Adapt `etc/lily-daemon.service` and copy to /etc/systemd/system then run:
```
sudo systemctl start lily-daemon.service
```

## Build the chainlink adapters
```
git clone https://github.com/alt-labs/external-adapters-js
cd external-adapters-js
yarn
yarn setup

docker-compose -f docker-compose.generated.yaml build lotus-adapter
docker-compose -f docker-compose.generated.yaml build lotus-msig-adapter
```
## Run the rest of the services (chainlink + adapters)

replace the ETH client URL with your own
```ini
ROOT=/chainlink
LOG_LEVEL=debug
ETH_CHAIN_ID=4
CHAINLINK_TLS_PORT=0
SECURE_COOKIES=false
ALLOW_ORIGINS=*
DATABASE_URL=postgres://postgres:hakabdoanoij312332123123maaamamaa@timescaledb:5432/chainlinkovan?sslmode=disable
ETH_URL=<ETH_CLIENT_WEBSOCKET_URL_HERE>
CHAINLINK_DEV=true
```
and put into `~/.chainlink-rinkeby/.env`


```
docker-compose up -d
```