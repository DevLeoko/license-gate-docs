---
title: License scope
layout: default
parent: Restriction options
nav_order: 1
---

# License scope

The license scope is used to define the scope of the license. This can be used
to differentiate between different kinds of licenses.

Most commonly, the license scope is used to differentiate between:

- Different software products
- Different versions of a software product
- Different features of a software product
- Different environments (e.g. development, staging, production)

The scope for a license is a string.

When validating a license, you can pass the scope in your request to
LicenseGate. LicenseGate will then check if the scope of the license matches the
scope in the request. If the scope does not match, the license verification will
fail (`LICENSE_SCOPE_FAILED`).
