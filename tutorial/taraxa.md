---
description: 'status: active - how to be in top block producer and get 2,000$TARA/ week'
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: false
---

# ✅ taraxa

## source

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>source: <a href="https://community.taraxa.io/bounties/56">https://community.taraxa.io/bounties/56</a></p></figcaption></figure>

## minimal requirements

* CPU: 4vCPU
* RAM: 8 GB
* DISK SPACE: 250 GB

## how to participate

follow these steps:

### register with the community site

[https://community.taraxa.io](https://community.taraxa.io)

### pass KYC, enter an ETH wallet

[https://community.taraxa.io/profile](https://community.taraxa.io/profile)

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### run a node

Guide for running the Taraxa Node with Docker on Linux:

#### install Docker

Open a terminal window and run the following commands to install Docker:

```
wget -O get-docker.sh https://get.docker.com 
sudo sh get-docker.sh
sudo apt install -y docker-compose
rm -f get-docker.sh
```

#### set up a Lite Consensus Node

```
mkdir -p testnet/config
cd testnet
wget https://raw.githubusercontent.com/Taraxa-project/taraxa-ops/master/taraxa_compose/docker-compose.light.yml -O docker-compose.yml
docker-compose up -d
docker-compose logs
```

You can have multiple nodes on the same IP address, but they need to occupy different ports. On the first node, you can just leave everything by default in the docker image.&#x20;

For subsequent nodes, you'll need to map the default ports to different ports (each node a different mapping, of course). For example, you may want to run the 2nd node, you should create a new folder like `$home/testnet2` and change port settings in the `docker-compose.yml` file as follow:

```
version: "3"
services:
  node:
    image: taraxa/taraxa-node:v1.5.0-beta
    restart: always
    ports:
      - "10003:10003"
      - "10003:10003/udp"
      - "7778:7778"
      - "8778:8778"
    entrypoint: /usr/bin/sh
    command: >
      -c "mkdir -p /opt/taraxa_data/data &&
          taraxad --chain testnet --wallet /opt/taraxa_data/conf/wallet.json --config /opt/taraxa_data/conf/testnet.json --data-dir /opt/taraxa_data/data --overwrite-config --light --enable-test-rpc"
    volumes:
      - ./config:/opt/taraxa_data/conf
      - data:/opt/taraxa_data/data
  status-app:
    image: taraxa/taraxa-node-status:latest
    environment:
      - NEXT_PUBLIC_RPC=http://node:7778
    restart: always
    depends_on:
      - node
    ports:
      - "3001:3001"
volumes:
  data:
```

### register your node to receive delegation

[https://community.taraxa.io/node](https://community.taraxa.io/node)

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### see if you’re on the top

[https://testnet.explorer.taraxa.io/nodes/](https://testnet.explorer.taraxa.io/nodes/)

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>
