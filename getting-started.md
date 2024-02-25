---
title: Getting Started
layout: default
nav_order: 2
---

<!-- prettier-ignore-start -->
# Getting Started
{: .no_toc }
<!-- prettier-ignore-end -->

<p class="fs-6 fw-300">
Getting started with LicenseGate is easy but your journey might look different
depending on your use case.
</p>

<!-- prettier-ignore -->
1. TOC 
{:toc}

## Sign Up or Self-Host

You can either sign up for a free account on LicenseGate Cloud or self-host
LicenseGate on your own infrastructure.

### Sign Up

To sign up for a free account you just need an email address and a password - no
credit card required.

[Create an account](https://app.licensegate.io/auth/signup){: .btn}

### Self-Host

For detailed instructions on how to self-host LicenseGate, please refer to the
[Self-Hosting Guide](/self-hosting).

## Create your first License

1. Navigate to ["Manage Licenses"](https://app.licensegate.io/licenses) and
   click on ["+ New License"](https://app.licensegate.io/licenses/new) on the
   top right.
2. Fill out all required fields
   1. Per default you get a random UUIDv4 as license key. You can change this to
      anything you like.
   2. The `Your Notes' section is optional and can be used to store additional
      information about the license.
   3. You can also specify various [restrictions](/restriction-options) for the
      license.
3. Click "Save" on the top right to create the license.

## Integrate LicenseGate into your application

Once you created your first license, you will get a popup that walks you through
various integration options.

Read more about [security considerations](/security-considerations) to
understand what you need to consider when integrating LicenseGate into your
application.

At the core a license check is a simple HTTP request:

```
GET https://api.licensegate.io/license/{user-id}/{license-key}/verify
```

You can find your `user-id` in the
[settings](https://app.licensegate.io/settings) of your LicenseGate account.

The server will respond with a json object that contains the validity of the
license:

```json
{
  "valid": true,
  "result": "VALID"
}
```
