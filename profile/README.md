Este é um projeto brasileiro procurando contornar a censura da internet, além do acesso gratuito para uma conexão VPN.

Este projeto tem vários programas para Tunelamento via SSH e pelo Wireguard.

## Mantenedores

- @Sirherobrine23 <srherobrine20@gmail.com>

## Iniciando o Servidor

Docker:

```shell
docker run --rm -d -e MongoDB_URL=<YouMongoConnectString> ghcr.io/ofvp-project/server:latest
```

Docker Composed:

```yml
version: "3.9"
volumes:
  OfvpServerData:
  MongoOFVp:
networks:
  ofvp_network:
    driver: bridge
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
  ofvp_server:
    container_name: ofvp_server
    restart: on-failure
    privileged: true
    image: ghcr.io/ofvp-project/server:latest
    depends_on:
      - ofvp_mongodb
    networks:
      - "ofvp_network"
    ports:
      - 3000:3000/tcp
      - 80:80/tcp
      - 51820:51820/udp
      - 2222:22/tcp
      - 2200:22/tcp
      - 7300:7300/tcp
      - 10086:10086/udp
      - 10086:10086/tcp
    volumes:
      - /lib/modules/:/lib/modules/
      - OfvpServerData:/root/OFVpServer
```
