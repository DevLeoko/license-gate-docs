---
title: Self hosting
layout: default
nav_order: 7
has_children: true
---

<!-- prettier-ignore-start -->
# Self hosting
{: .no_toc }
<!-- prettier-ignore-end -->

<!-- prettier-ignore -->
1. TOC 
{:toc}

The LicenseGate [GitHub repository](https://github.com/DevLeoko/license-gate) is
a monorepo containing both the frontend and backend code. The frontend is built
with SvelteKit and the backend is a NodeJS Express server.

## Prerequisites

- VPS with NodeJS >= 16.x installed
- MySQL database
- SMTP server

## Architecture

LicenseGate consists of two parts:

**The frontend** is the web interface that users interact with. It is a static
site that can be hosted on any static file server (e.g. Nginx, Cloudflare Pages,
...). The frontend is not a running server, it is just a collection of static
files.

**The backend** is the server that the frontend communicates with and that
performs the verification of licenses. The backend is a NodeJS Express server
that can be hosted anywhere that supports NodeJS (e.g. Your own VPS, Heroku,
Vercel, ...).

The frontend and backend need to be served under separate domains or subdomains.
For example, the frontend should be served under
`https://licensegate.example.com` and the backend under
`https://api.licensegate.example.com`.

## Frontend

1. Clone the [GitHub repository](https://github.com/DevLeoko/license-gate): 
`git clone https://github.com/DevLeoko/license-gate`, which creates a folder 
    named "license-gate"
2. Navigate to the frontend directory: `cd frontend`.
3. Setup the environment variables in `.env` (see below for an example). For
   security reasons the `.env` file in the `backend` directory should be empty
   when building the frontend.
4. Run `npm install` and `npm run build`
5. The build output is in the `frontend/build` directory.

The files should be served directly under a domain or subdomain and not in a
subdirectory. For example, the files should be served under
`https://licensegate.example.com` and not `https://example.com/licensegate`.

### Serving the files with Nginx

1. Create a new folder for the frontend files: `mkdir -p /var/www/licensegate`
2. Copy the files to the new folder:
   `cp -r frontend/build/* /var/www/licensegate`
3. Create a new Nginx server block:
   `nano /etc/nginx/sites-available/licensegate`
4. Add the following configuration to the server block:

```nginx
server {
    listen 80;
    server_name licensegate.example.com;

    root /var/www/licensegate;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

5. Enable the server block:
   `ln -s /etc/nginx/sites-available/licensegate /etc/nginx/sites-enabled/licensegate`
6. Reload Nginx: `systemctl reload nginx`
7. The frontend should now be accessible at `https://licensegate.example.com`

### Environment variables

| Variable name                  | Description                                                                                                                 | Example                              |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| `PUBLIC_BACKEND_URL`           | The URL of the backend server                                                                                               | `https://api.licensegate.io`         |
| `PUBLIC_RECAPTCHA_SITE_KEY`    | The site key for reCAPTCHA (v2 - invisible). This is used to protect signup and password-rest pages                         | `6Lc...`                             |
| `PUBLIC_GOOGLE_AUTH_CLIENT_ID` | The client ID for Google OAuth. This is used to allow users to sign in with their Google account. Set to `none` to disable. | `1234....apps.googleusercontent.com` |

```
PUBLIC_BACKEND_URL=https://api.licensegate.example.com
PUBLIC_RECAPTCHA_SITE_KEY=6Lc...
PUBLIC_GOOGLE_AUTH_CLIENT_ID=none
```

### Hosting the frontend on Cloudflare Pages

The easiest way to host the frontend is to use Cloudflare Pages. (These steps do
not require the steps above)

1. Fork the [GitHub repository](https://github.com/DevLeoko/license-gate)
2. Sign in to your Cloudflare account and go to "Workers & Pages"
3. Click "Create application"
4. Select "Pages" and then "Connect to Git"
5. Select the forked repository
6. Set these build settings:
   - Build command: `cd frontend && npm i && npm run build`
   - Build output directory: `frontend/build`
   - Environment variables: As described below

## Backend

1. Clone the [GitHub repository](https://github.com/DevLeoko/license-gate): `git clone https://github.com/DevLeoko/license-gate`, which creates a folder 
    named "license-gate"
2. Navigate to the backend directory: `cd backend`.
3. Setup the environment variables `.env` (see below for an example)
4. Run `npm install`, `npm run prisma-up` and `npm run prisma-gen` to setup the
   database and generate the Prisma client.
5. Run `npm run start` to start the backend server.

### Environment variables

| Variable name           | Description                                                                                                                 | Example                                         |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------- |
| `NODE_ENV`              | The environment the server is running in. Set to `production` in production.                                                | `production`                                    |
| `HEX_USER_ID_OFFSET`    | The offset to add to the user ID when converting it to a hex string. You can set this to `0`.                               | `a3f`                                           |
| `PORT`                  | The port the server should listen on.                                                                                       | `3000`                                          |
| `DATABASE_URL`          | The URL to the MySQL database.                                                                                              | `mysql://user:password@host/db`                 |
| `SMTP_HOST`             | The hostname of the SMTP server.                                                                                            | `smtp.example.com`                              |
| `SMTP_USERNAME`         | The username for the SMTP server.                                                                                           | `user`                                          |
| `SMTP_PASSWORD`         | The password for the SMTP server.                                                                                           | `password`                                      |
| `SMTP_PORT`             | The port of the SMTP server.                                                                                                | `587`                                           |
| `SMTP_SENDER`           | The email address that should be used as the sender.                                                                        | `LicenseGate <noreply@domain.com>`              |
| `JWT_SECRET`            | The secret used to sign the JWT tokens. (any random string will do)                                                         | `secret`                                        |
| `RECAPTCHA_SECRET_KEY`  | The secret key for reCAPTCHA (v2 - invisible). This is used to protect signup and password-rest pages                       | `a4c...`                                        |
| `SIGN_IN_URL`           | The URL of the frontend sign in page.                                                                                       | `https://licensegate.example.com/auth/login`    |
| `RESET_PASSWORD_URL`    | The URL of the frontend reset password page.                                                                                | `https://licensegate.example.com/auth/password` |
| `CORS_ORIGIN`           | The frontend origin that is allowed to access the server. Set to `*` to allow any origin.                                   | `https://licensegate.example.com`               |
| `GOOGLE_AUTH_CLIENT_ID` | The client ID for Google OAuth. This is used to allow users to sign in with their Google account. Set to `none` to disable. | `1234....apps.googleusercontent.com`            |

```
NODE_ENV=production
HEX_USER_ID_OFFSET=0
PORT=3000
DATABASE_URL="mysql://user:password@host/db"
SMTP_HOST=smtp.example.com
SMTP_USERNAME=user
SMTP_PASSWORD=password
SMTP_PORT=587
SMTP_SENDER="LicenseGate <noreply@domain.com>"
JWT_SECRET=secret
RECAPTCHA_SECRET_KEY=a4c...
SIGN_IN_URL=https://licensegate.example.com/auth/login
RESET_PASSWORD_URL=https://licensegate.example.com/auth/password
CORS_ORIGIN=https://licensegate.example.com
GOOGLE_AUTH_CLIENT_ID=none
```

## Usage with wrappers
LicenseGate wrapper libraries, by default, use the public LicenseGate server which is provided under `api.licensegate.io`. In order to use your self-hosted licensing server, you will need to configure your server url when using wrappers.
Refer to the wrapper documentation for detailed instructions.
