---
title: Rate Limit
layout: default
parent: Restriction options
nav_order: 3
---

# Rate limit

The rate limit is used to limit the number of requests that can be made to
LicenseGate to verify this license in a specific time period.

For rate limiting you can specify the following parameters:

- **Requests per time period**: The number of requests that can be made in the
  specified time period on average (e.g. 100 requests per hour).
- **Time period**: The time period in which the requests are counted (e.g. 1
  hour).
- **Burst**: The maximum number of requests that can be made, even exceeding the
  average rate limit

For the rate limiting algorithm, LicenseGate uses a token bucket algorithm [^1].

Every `time period` (e.g. every hour) the license is refilled with
`requests per time period` tokens (e.g. 100 tokens). Up to `burst` tokens can be
accumulated in the bucket. When a request is made, a token is removed from the
bucket. If no token is available, the request is rejected. Invalid requests do
not remove tokens from the bucket.

This rate limiting allows for short bursts of requests that exceed the average
rate limit. This can be useful to handle short peaks of requests.

When the rate limit is exceeded, the license verification will fail
(`RATE_LIMIT_EXCEEDED`).

[^1]: [Wikipedia: Token bucket](https://en.wikipedia.org/wiki/Token_bucket)
