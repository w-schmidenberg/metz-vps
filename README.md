# Home Server

## Deploying:

1. Init Docker Swarm

```sh
docker swarm init --advertise-addr {IP-ADDR-SERVER}
```

You will see
```sh
docker swarm join --token SWMTKN-1-1ld6ytebe7q2np3cshbd6g1l5d00j8ybo537blivmbwt2culef-6hwrs4vy35i6qo0sz9r5q2aj4 {IP-ADDR-SERVER}:2377
```

### 1-Proxy

1.Create networks

```sh
docker network create --driver overlay --attachable proxy_socket_access_network
docker network create --driver overlay --attachable proxy_internet_access_network
docker network create --driver overlay --attachable security_network
```

2.Create volumes

```sh
docker volume create 1-traefik_certs
docker volume create 1-traefik_dumped_certs
docker volume create 1-traefik_acme_data
docker volume create 1-traefik_log_data
```

3. Deploy

```sh
docker stack deploy -c 1-proxy.yml 1-proxy --detach=true
```

### 2-Infra

```sh
docker node update --label-add portainer.portainer-data=true $(docker info -f '{{.Swarm.NodeID}}')
docker volume create 2-portainer_data
```
```sh
docker stack deploy -c 2-infra.yml 2-infra --detach=true
```

### 3-Vault
```sh
docker volume create 3-vaultwarden_data
```