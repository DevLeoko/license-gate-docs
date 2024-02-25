---
title: Security considerations
layout: default
nav_order: 3
---

<!-- prettier-ignore-start -->
# Security considerations
{: .no_toc }
<!-- prettier-ignore-end -->

<p class="fs-6 fw-300">
What are the security implications of different integration options? 
And what are the inherent limitations when licensing software?
</p>

<!-- prettier-ignore -->
1. TOC 
{:toc}

## Integration options

How secure is licensing your software with LicenseGate actually? **Within its
limitations, LicenseGate is very secure**. Meaning that everything that is
within the control of LicenseGate is designed to be as secure as possible. But
there are also inherent limitations to licensing software that you should be
aware of. Let's dive into the details.

### License checks in the backend

Are you performing license checks in the backend of your application? This is
the most secure way to license your software. The security is now only limited
by the security of your backend and business logic.

### License checks in the frontend

This where things get a bit more tricky. If you are performing license checks in
the frontend of your application (i.e. on the client's device) there are two primary
security concerns:

1. Manipulation of the client-side code to bypass the license check
2. Manipulation of the traffic between the client and LicenseGate

