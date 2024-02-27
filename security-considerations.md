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

How secure is licensing your software with LicenseGate actually? **Within its
limitations, LicenseGate is very secure**. Meaning that everything that is
within the control of LicenseGate is designed to be as secure as possible. But
there are also inherent limitations to licensing software that you should be
aware of. Let's dive into the details.

## License checks in the backend

Are you performing license checks in the backend of your application? This is
the most secure way to license your software. The security is now only limited
by the security of your backend and business logic.

## License checks in the frontend

This where things get a bit more tricky. If you are performing license checks in
the frontend of your application (i.e. on the client's device) there are two
primary security concerns:

1. Manipulation of the client-side code to bypass the license check
2. Manipulation of the traffic between the application and LicenseGate

First we need to understand how a typical request flow looks like when you are
performing license checks in the frontend.

Your application is running on the client's device. Within your code you have
logic that performs the license check. To verify a license this logic sends a
request from the client's device to the LicenseGate server. The LicenseGate
server then responds with the result of the verification. Your application then
receives this response and acts accordingly.

![default request flow](/assets/request-schema.svg)

### Manipulation of the client-side code

From a security perspective, the client has full control over the client-side
code. This means that the client can manipulate the code to bypass the license
check. This is a fundamental limitation of licensing software in the frontend.

![manipulation of client-side code](/assets/attack01.svg)

#### Risk & mitigation

- To make your code harder to manipulate you can obfuscate your code. This makes
  it harder for the client to understand and manipulate the code. But keep in
  mind that this is only a mitigation and not a solution.
- Compiled languages are generally harder to manipulate than interpreted
  languages.
- More complex applications are generally harder to manipulate than simple
  applications.
- Also see [General risk factors](#general-risk-factors)

### Manipulation of the traffic between the application and LicenseGate

When manipulation of the client-side code is not feasible, the next attack
vector is the traffic between the application and LicenseGate. The client can
manipulate the traffic to make it look like the license check was successful.

![manipulation of traffic](/assets/attack02.svg)

#### Using LicenseGate's RSA challenge-response mechanism

These attacks can be prevented by using LicenseGate's RSA challenge-response
mechanism. For every license check, your application also sends a challenge (we
recommend the current time in milliseconds) to the LicenseGate server. Only the
LicenseGate server can correctly sign this challenge and by validating the
signature in your application you can be sure that the response is authentic and
has not been manipulated.

#### What about HTTPS?

You might be thinking: "But I'm using HTTPS, so the traffic is already encrypted
and cannot be manipulated". This is true, if the attacker were to be in between
the client and LicenseGate (man-in-the-middle attack). But as the attacker is
the client himself, he can freely manipulate the traffic regardless of SSL
certificates.

### General risk factors

The real risk of attacks on your licensing logic also depends on a number of
other factors that should be considered:

- The technical knowledge of your users
- Operating system and device (e.g. IOS apps are generally speaking harder to
  manipulate than web apps)
- The overall complexity of the application
- The value of the software (e.g. a software that costs 1000$ is more likely to
  be manipulated than a software that costs 10$)

So for example when developing a gardening app that you sell for $3.99 on the
app store, the risk of attacks on your licensing logic is generally low and you
have to evaluate the cost-benefit-time ratio of advanced security measures.
