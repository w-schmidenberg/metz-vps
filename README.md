# Metz VPS Server

## Server Setup
Start by reading [0-VPS_Setup.md](https://github.com/Thaloz/metz-vps/blob/main/0-VPS_Setup.md)

## Deploying:

1. Init Docker Swarm

```sh
docker swarm init --advertise-addr {IP-ADDR-SERVER}
```

You will see
```sh
docker swarm join --token {YOUR-TOKEN} {IP-ADDR-SERVER}:2377
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

### Portainer and other stacks.
From this point on, you must deploy all the other stacks through Portainer (2-infra).

The usual steps are the following:
1. Go into the stack.yml file and take note of external networks, volumes or secrets that must be created.
2. Log into Portainer and create the necessary resources.
3. `Portainer > Stack > Deploy` and copy and paste the content of the *.yml you want to deploy (ex. `3-vault.yml`)
4. Add the Environment Variables needed.
5. Deploy

## Authentik and extra config
- After all the stacks are deployed, you should setup Authentik and enable the Forward Auth proxies for security and OIDC for SSO OAuth login.
- Most of the documentation can be found in the official website, ex: https://docs.goauthentik.io/integrations/services/portainer/

## Vaultwarden
- You can setup Bitwarden Browser Extension and point it at a self-hosted instance.

## Security
- At the very basic level we should setup Authentik with Forward Auth to ensure only logged in users can assess the services.
- As a next step, we should deploy [Crowdsec](https://www.crowdsec.net/)