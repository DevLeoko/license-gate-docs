---
title: SMTP-Server
layout: default
nav_order: 1
parent: Self hosting
---

# SMTP
SMTP stands for Simple Mail Transfer Protocol. It is what your selfhosted LicenseGate tool will use to be able to use the following features:
- Register new accounts (Sending the verification email)
- Request password resets (Sending the password-reset link  )

## Using GoogleMail as your SMTP provider
Gmail allows you to use their SMTP Server to send mails. [Read more about it here](https://support.google.com/a/answer/176600?hl=en#:~:text=465%2C%20or%20587.-,Option%202%3A%20Send%20email%20with%20the%20Gmail%20SMTP%20server,-If%20you%20connect)

**Before using Gmail as your SMTP Server, please understand the risks and disadvantages over using a provider that does it for you.** [Read more here](https://support.google.com/accounts/answer/6010255?hl=en&sjid=1195126002269736074-EU)

To use Gmail as your SMTP server, you need to set up your application with the following SMTP details:

- SMTP server: `smtp.gmail.com`
- Username: `Your Gmail address (e.g., user@gmail.com)`
- Password: `Your Gmail password`
- Port (TLS): `587`
- Port (SSL): `465`
- TLS/SSL required: `yes`

However, to use Gmail's SMTP server, you may need to allow "Less secure apps" in your Google account settings, which could make your account more vulnerable.

Here's a step-by-step guide:

- Log in to your Google account.
- Go to the "Less secure apps" section of your account. You can find it here: [https://myaccount.google.com/lesssecureapps](https://myaccount.google.com/lesssecureapps)
- Turn on "Allow less secure apps".
- After doing this, you should be able to use Gmail as your SMTP server.

## Using a mail provider
Here are a few mail providers to look into:
- [Postmark](https://postmarkapp.com/)
- [Sendgrid](https://sendgrid.com)
- [Mailgun](https://mailgun.com)
- [MessageBird](https://bird.com)

## Selfhosting a mailserver
Another solution in getting a mailserver for your panel (and for emailing in general as a bonus-point) is to use [Mailcow](https://mailcow.email/):

> mailcow: dockerized is an open source groupware/email suite based on docker.

**Keep in mind that this will require you to provide the following:**
- Selfhost a webserver for the panel
- Own a domain name (such as example.com)

The documentation about setting up Mailcow can be found at [docs.mailcow.email](https://docs.mailcow.email/).