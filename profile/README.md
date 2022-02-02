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
  HttpInjectorData:
networks:
  ofvp_network:
    enable_ipv6: true
services:
  ofvp_server:
    container_name: ofvp_server
    restart: on-failure
    privileged: true
    image: ghcr.io/ofvp-project/server:latest
    depends_on:
      - ofvp_mongodb
    networks:
      - "ofvp_network"
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
      - net.ipv6.conf.all.disable_ipv6=0
      - net.ipv6.conf.all.forwarding=1
      - net.ipv4.ip_forward=1
    environment:
      MongoDB_URL: "<MongoDB String>"
    ports:
      - 3000:3000/tcp
      - 80:80/tcp
      - 51820:51820/udp
      - 22:22/tcp
      - 2200:22/tcp
      - 7300:7300/tcp
      - 10086:10086/udp
      - 10086:10086/tcp
    volumes:
      - /lib/modules/:/lib/modules/
      - HttpInjectorData:/root/OFVpServer
```
