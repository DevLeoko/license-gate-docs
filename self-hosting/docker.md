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

#### Disclaimer
**The use of the docker-based setup of licensegate may come with risks, depending on how it is configured. The maintainers of LicenseGate or contributors of the docker-version are not responsible for issues this may cause on either your system or the data stored within it.**

It is recommended to change the configurations to not match the default settings. Included in these are database credentials for example.

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
Docker-based LicenseGate requires setting up the configurations within the `.env` file in the main folder (based off of the template `.env.docker.example`).

```bash
## Docker-Configuration for using LicenseGate within a docker-environment
## !!! These are not the main environment variables to use when setting up licensegate without docker !!! 
## Refer to the docs for more information

## SMTP ##
# The SMTP server is where mail will be sent from. This is required for registering and password resets.
SMTP_HOST=mail.example.com
SMTP_PORT=465
SMTP_USERNAME=licensegate@example.com
SMTP_PASSWORD=mySecr4tPassw0rd
SMTP_SENDER=LicenseGate <licensegate@example.com>

# Sessioning-secret, can be left default
JWT_SECRET=secret

## Recaptcha: Anti-spam for registering ##
## Obtain one here: https://www.google.com/recaptcha/admin/create
PUBLIC_RECAPTCHA_SITE_KEY=XYZABCDEFGH # "Use this site key in the HTML code your site serves to users."
RECAPTCHA_SECRET_KEY=XYZABCDEFGH # "Use this secret key for communication between your site and reCAPTCHA."

# Google Auth
PUBLIC_GOOGLE_AUTH_CLIENT_ID=none

PUBLIC_BACKEND_URL=https://licensegate-api.example.com
PUBLIC_FRONTEND_URL=https://licensegate.example.com

## Database (only MySQL supported) ##
MYSQL_HOSTNAME=db # Unless you're providing your own database, do not change this
MYSQL_PORT=3306
MYSQL_DATABASE=license-gate
MYSQL_ROOT_PASSWORD=9Wu7vxKjwQPo2YyFKqTYbv7CZ 
MYSQL_USER=mysqluser
MYSQL_PASSWORD=nDdt2uMRJgov2om2dwVYRWkDm

# Required for certificate generation on caddy, these must match your PUBLIC_BACKEND_URL and PUBLIC_FRONTEND_URL
BACKEND_FQDN=licensegate-api.example.com
FRONTEND_FQDN=licensegate.example.com
```

The `BACKEND_FQDN` and `FRONTEND_FQDN` are required for ssl-certificate generation and **NEED** to match your frontend and backend urls. These are the domains your dashboard and the associated API will be reachable from.

## Starting the docker-service

Running the following command to start your LicenseGate dashboard:
```bash
# -d to start in detached mode
docker compose up -d
```

## Viewing your dashboard

Please wait up to a minute for the dashboard to fully start up, including its database and API.

You can now visit the dashboard under your `PUBLIC_FRONTEND_URL`.

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

```bash
# Backend
...
PUBLIC_BACKEND_URL=https://licensegate-api.example.com:4443 # change ports here by adding :port-number
PUBLIC_FRONTEND_URL=https://licensegate.example.com:4443 # change ports here by adding :port-number
...
# Frontend
PUBLIC_BACKEND_URL=https://licensegate-api.example.com:4443 # change ports here by adding :port-number
```

**Keep in mind that renewing certificates may cause issues when changing ports. If you wish to renew certificates, change the port back to the previous one, then generate new ones.**
