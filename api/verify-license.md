---
title: Verify License
layout: default
parent: API
nav_order: 1
---

<!-- prettier-ignore-start -->
# Verify License
{: .no_toc }
<!-- prettier-ignore-end -->

The `verify-license` endpoint is used to verify a license.

<!-- prettier-ignore -->
1. TOC 
{:toc}

## Request

<dl>
  <dt>Method</dt>
  <dd>GET or POST</dd>
  <dt>URL</dt>
  <dd><code>https://api.licensegate.com/license/{user-id}/{license-key}/verify</code></dd>
</dl>

### Parameters

#### Path parameters

<dl>
  <dt>user-id</dt>
  <dd>The user ID of the license owner - you can find this in your <a href="https://app.licensegate.io/settings/account">account settings</a>.</dd>
  <dt>license-key</dt>
  <dd>The license key to verify.</dd>
</dl>

#### Query / Body parameters

These parameters can be passed as query parameters for GET requests or as JSON
in the body for POST requests.

<dl>
  <dt>scope</dt>
  <dd>
    The scope of the license. Required if the license has a <a href="restriction-options/scope">scope restriction</a>
    <br> 
    <span class="label label-blue">Optional</span> 
  </dd>
  <dt>challenge</dt>
  <dd>
    A challenge that will be signed by the server. We recommend to use the current time in milliseconds as the challenge. See <a href="security-considerations">Security considerations</a> for more information.
    <br> 
    <span class="label label-blue">Optional</span>
  </dd>
  <dt>metadata</dt>
  <dd>
    A string that will be logged with the license verification. This can be used to log additional information about the verification request.
    <br>
    <span class="label label-blue">Optional</span>
  </dd>
</dl>

## Response

The response will be a JSON object with the following properties:

<dl>
  <dt>valid</dt>
  <dd>
    Indicating if the license is valid (short for <code>result == true</code>)
    <br>
    <span class="label label-green">boolean</span>
  </dd>
  <dt>result</dt>
  <dd>
    The result of the verification. Possible values are:
    <ul>
      <li><code>VALID</code>: The license is valid</li>
      <li><code>NOT_FOUND</code>: The license was not found</li>
      <li><code>NOT_ACTIVE</code>: The license is not active</li>
      <li><code>EXPIRED</code>: The license has expired</li>
      <li><code>LICENSE_SCOPE_FAILED</code>: The scope of the license does not match the scope in the request</li>
      <li><code>IP_LIMIT_EXCEEDED</code>: The IP limit has been exceeded</li>
      <li><code>RATE_LIMIT_EXCEEDED</code>: The rate limit has been exceeded</li>
    </ul>
    <span class="label label-green">string</span>
  </dd>
  <dt>signedChallenge</dt>
  <dd>
    The challenge signed by the server. Only present if a challenge was provided in the request.
    <br>
    <span class="label label-green">string</span>
  </dd>
</dl>

### Error responses

If the request schema is invalid, the server will respond with a 400 Bad
Request:

```json
{
  "valid": false,
  "error": "INVALID_REQUEST_SCHEMA"
}
```

If there is any other internal error, the server will respond with a 500
Internal Server Error:

```json
{
  "valid": false,
  "error": "INTERNAL_SERVER_ERROR"
}
```

## Examples

### Request

```http
GET /license/123/abc-123/verify HTTP/1.1
Host: api.licensegate.com
```

### Response

```json
{
  "valid": true,
  "result": "VALID"
}
```

---

### Request

```http
GET /license/123/abc-123/verify?scope=premium-plus&challenge=1234567890 HTTP/1.1
Host: api.licensegate.com
```

### Response

```json
{
  "valid": true,
  "result": "VALID",
  "signedChallenge": "23fa25/7dd..."
}
```

### Response

```json
{
  "valid": false,
  "result": "EXPIRED"
}
```

### Response

```json
{
  "valid": false,
  "result": "LICENSE_SCOPE_FAILED"
}
```
