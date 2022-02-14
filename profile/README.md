Este é um projeto brasileiro procurando contornar a censura da internet, além do acesso gratuito para uma conexão VPN.

Este projeto tem vários programas para Tunelamento via SSH e pelo Wireguard.

## Mantenedores

- @Sirherobrine23 <srherobrine20@gmail.com>

## Iniciando o Servidor

Docker Composed:

```yml
version: "3.9"
volumes:
  ofvp_storage:
  MongoOFVp:
networks:
  ofvp_network:
    enable_ipv6: true
services:
  ofvp_mongodb:
    container_name: ofvp_mongodb
    restart: on-failure
    image: mongo
    volumes:
      - MongoOFVp:/data/db
    networks:
      - "ofvp_network"
    environment:
      - PUID=1000
      - PGID=1000
    entrypoint:
    - "mongod"
    - "--bind_ip_all"
    - "--quiet"
    - "--logpath"
    - "/dev/null"
  ofvp_server:
    container_name: ofvp_server
    restart: on-failure
    image: ghcr.io/ofvp-project/server:latest
    depends_on:
      - ofvp_mongodb
    networks:
      - "ofvp_network"
    volumes:
      - /lib/modules/:/lib/modules/
      - ofvp_storage:/root/OFVpServer
    environment:
      MongoDB_URL: "mongodb://ofvp_mongodb:27017"
      START_OPENSSH: "1"
      START_WIREGUARD: "1"
    ports:
      - 3000:3000/tcp
      - 2222:22/tcp
      - 2200:22/tcp
      - 51820:51820/udp
    privileged: true
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
      - net.ipv6.conf.all.disable_ipv6=0
      - net.ipv6.conf.all.forwarding=1
      - net.ipv4.ip_forward=1
```
