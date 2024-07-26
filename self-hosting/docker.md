---
title: Setup with Docker
layout: default
nav_order: 2
parent: Self hosting
---

# Docker

LicenseGate can alternatively be hosted using docker.

## Setup

### Installing docker
Docker can be installed by following the documentation from docker itself [here](https://docs.docker.com/engine/install/).

### Setting up the environment
**Setting up the caddy Network**: Docker-based LicenseGate uses [caddy-docker-proxy](https://github.com/lucaslorentz/caddy-docker-proxy), 
which requires setting up a network within docker with the name "caddy":
```bash
docker network create caddy
```

**Clone Repository**: Once the network has been created, the repository can be cloned using the following command:
```bash
git clone https://github.com/DevLeoko/license-gate
```

**Configure settings through .env**:
Docker-based LicenseGate requires setting up the configurations within the .env on the main folder (based off of the template .env-example), aswell as in the frontend .env (based off of the template .env.example).

#### Main .env
```
SMTP_HOST=mail.example.com
SMTP_USERNAME=licensegate@example.com
SMTP_PASSWORD=mailpass
SMTP_SENDER=LicenseGate <licensegate@example.com>
JWT_SECRET=secret
RECAPTCHA_SECRET_KEY=XYZABC
PUBLIC_BACKEND_URL=https://licensegate-api.example.com
PUBLIC_FRONTEND_URL=https://licensegate.example.com
MYSQL_ROOT_PASSWORD=rootpassword # Make sure to change this
MYSQL_DATABASE=mydb
MYSQL_USER=user
MYSQL_PASSWORD=password # Make sure to change this
BACKEND_FQDN=licensegate-api.example.com
FROTNEND_FQDN=licensegate.example.com
```

#### Frontend .env
```
PUBLIC_BACKEND_URL=https://licensegate-api.example.com
PUBLIC_RECAPTCHA_SITE_KEY=XYZABC
PUBLIC_GOOGLE_AUTH_CLIENT_ID=none
```

The Backend_FQDN and FRONTEND_FQDN are required for certificate generation and **NEED** to match your frontend and backend urls.

## Starting the docker-service

Running the following command to start your LicenseGate Dashboard:
```bash
# -d to start in detached mode
docker compose up -d
```

## Viewing your Dashboard

Please wait up to a minute for the dashboard to fully start up, including its database and API.

You can now visit the dashboard under your ``PUBLIC_FRONTEND_URL``.

