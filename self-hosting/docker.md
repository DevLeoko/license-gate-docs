---
title: Setup with Docker
layout: default
nav_order: 2
parent: Self hosting
---

# Docker

LicenseGate can alternatively be hosted using docker.

Thanks to [CurlyBytes](https://github.com/CurlyBytes) for [contributing](https://github.com/DevLeoko/license-gate/pull/3) towards bringing LicenseGate to docker.

## Setup

### Installing docker
Docker can be installed by following the documentation from docker itself [here](https://docs.docker.com/engine/install/).

### Setting up the environment
**Setting up the caddy network**: Docker-based LicenseGate uses [caddy-docker-proxy](https://github.com/lucaslorentz/caddy-docker-proxy), 
which requires setting up a network within docker with the name "caddy":
```bash
docker network create caddy
```

**Clone Repository**: Once the network has been created, the repository can be cloned using the following command:
```bash
git clone https://github.com/DevLeoko/license-gate.git
```

**Configure settings through .env**:
Docker-based LicenseGate requires setting up the configurations within the .env on the main folder (based off of the template .env.docker.example).

#### Base .env
```
# Backend
SMTP_HOST=mail.example.com
SMTP_USERNAME=licensegate@example.com
SMTP_PASSWORD=mySMTPPassw0rd
SMTP_SENDER=LicenseGate <licensegate@example.com>
JWT_SECRET=secret
RECAPTCHA_SECRET_KEY=XXYYZZAABBCCDD
PUBLIC_BACKEND_URL=https://licensegate-api.example.com
PUBLIC_FRONTEND_URL=https://licensegate.example.com
MYSQL_ROOT_PASSWORD=rootpassword
MYSQL_DATABASE=mydb
MYSQL_USER=user
MYSQL_PASSWORD=password
BACKEND_FQDN=licensegate-api.example.com
FROTNEND_FQDN=licensegate.example.com

# Frontend
PUBLIC_BACKEND_URL=https://licensegate-api.example.com
PUBLIC_RECAPTCHA_SITE_KEY=XXYYZZAABBCCDD
PUBLIC_GOOGLE_AUTH_CLIENT_ID=none
```

The Backend_FQDN and FRONTEND_FQDN are required for ssl-certificate generation and **NEED** to match your frontend and backend urls. These are the domains your dashboard and the associated API will be reachable from.

## Starting the docker-service

Running the following command to start your LicenseGate dashboard:
```bash
# -d to start in detached mode
docker compose up -d
```

## Viewing your dashboard

Please wait up to a minute for the dashboard to fully start up, including its database and API.

You can now visit the dashboard under your ``PUBLIC_FRONTEND_URL``.

## Advanced Users

### Running LicenseGate on a different port

LicenseGate will require to run on a https port per default. In order to change that port, you need to at least **ONCE** run the dashboard for the certificates to be generated.

Before changing ports, the docker containers have to be fully shutdown.

Changing the ports can be done the following:

#### Changing ports on docker-compose.yml:
```yaml
...
  caddy:
    image: lucaslorentz/caddy-docker-proxy:ci-alpine
    ports:
      - 80:80 # change first set of numbers here (ex. 8080:80)
      - 443:443 # change first set of numbers here (ex. 8443:443)
    environment:
      - CADDY_INGRESS_NETWORKS=caddy
    networks:
      - caddy
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - caddy_data:/data
    restart: unless-stopped
...
```

#### Changing ports within .env variables

Changing elements within the .env variables require a full rebuild of the images. This can be done by shutting down the containers using `docker compose down --rmi local` to remove relevant images. **Volumes are not deleted unless `-v` is used too**.

```
# Backend
...
PUBLIC_BACKEND_URL=https://licensegate-api.example.com:4443 # change ports here by adding :port-number
PUBLIC_FRONTEND_URL=https://licensegate.example.com:4443 # change ports here by adding :port-number
...
# Frontend
PUBLIC_BACKEND_URL=https://licensegate-api.example.com:4443 # change ports here by adding :port-number
```

**Keep in mind that renewing certificates may cause issues when changing ports. If you wish to renew certificates, change the port back to the previous one, then generate new ones.**
