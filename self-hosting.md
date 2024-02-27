---
title: Self hosting
layout: default
nav_order: 7
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

## Frontend

We do not use any SSR in SvelteKit, so the frontend can be hosted as a static
site. To build the frontend, you need to:

1. Setup the environment variables in `frontend/.env` (see `.env.example` for an
   example)
2. Run `npm install` and `npm run generate` in the `backend` directory. Note:
   The backend is required to build the frontend because the frontend uses the
   backend's tRPC types. For security reasons the backend's `.env` file should
   not have any sensitive data when building the frontend.
3. Run `npm install` and `npm run build` in the `frontend` directory.
4. The frontend is now built and can be hosted as a static site. The output is
   in the `frontend/build` directory.

## Backend

1. Navigate to the `backend` directory.
2. Setup the environment variables `.env` (see `.env.example` for an example)
3. Run `npm install`, `npm run prisma-up` and `npm run generate` to setup the
   database and generate the Prisma client.
4. Run `npm run start` to start the backend server.
