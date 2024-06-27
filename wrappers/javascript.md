---
title: JavaScript
layout: default
parent: Wrappers
nav_order: 2
---

<!-- prettier-ignore-start -->
# LicenseGate Wrapper for JavaScript
{: .no_toc }
<!-- prettier-ignore-end -->

<!-- prettier-ignore -->
1. TOC 
{:toc}

## Installation

**NPM:** [package/licensegate](https://www.npmjs.com/package/licensegate)   
**GitHub:** [Bensonheimer992/LicenseGate-JS-Wrapper](https://github.com/Bensonheimer992/LicenseGate-JS-Wrapper)

*This wrapper is not maintained by developers of licensegate, credits go towards [Bensonheimer992](https://github.com/Bensonheimer992).* 

To install LicenseGate Wrapper, simply run the following command:

### Installation with Node.js package manager  
```bash
npm install licensegate
```

## Dependencies
- Axios
- Node.js v20.11.10 or higher

## Usage
### Simple implementation   
```js
//index.js
const LicenseGate = require('licensegate').default;
const { ValidationType } = require('licensegate');

async function checkLicense() {
  const userId = 'Your UserID';
  const licenseKey = 'Your LicenseKey';

  const licenseGate = new LicenseGate(userId);

  const result = await licenseGate.verify(licenseKey);

  if (result === ValidationType.VALID) {
    console.log('The Key is Valid.');
  } else {
    console.log(`The Key is invalid. Reason: ${result}`);
  }
}

checkLicense().catch(console.error);
```

### With Custom Server URL
```js
const userId = 'Your UserID';
const licenseKey = 'Your LicenseKey';
const serverUrl = 'https://mybackendserver.com';

const licenseGate = new LicenseGate(userId).setValidationServer(serverUrl);

const result = await licenseGate.verify(licenseKey);

if (result === ValidationType.VALID) {
  console.log('The Key is Valid.');
} else {
    console.log(`The Key is invalid. Reason: ${result}`);
}
```

### With Public Rsa Key
```js
const userId = 'Your UserID';
const licenseKey = 'Your LicenseKey';
const publicRSAKey = 'Your RSA Key';

const licenseGate = new LicenseGate(userId).setPublicRsaKey(publicRSAKey).useChallenges();
// Alternatively:
// const licenseGate = new LicenseGate(userId, publicRSAKey).useChallenges();

const result = await licenseGate.verify(licenseKey);

if (result === ValidationType.VALID) {
  console.log('The Key is Valid.');
} else {
  console.log(`The Key is invalid. Reason: ${result}`);
}
```

### Enabling debugging for verbose output
```js
const userId = 'Your UserID';
const licenseKey = 'Your LicenseKey';

const licenseGate = new LicenseGate(userId).debug(); // Enabling debugging using .debug();

const result = await licenseGate.verify(licenseKey);

if (result === ValidationType.VALID) {
  console.log('The Key is Valid.');
} else {
  console.log(`The Key is invalid. Reason: ${result}`);
}
```

```
> node .
Sending request to URL : https://api.licensegate.io/license/XXXXX/dc47396c-XXXX-XXXX-XXXX-XXXXXXXXXXXX/verify
Response Code : 200
Response: [object Object]
The Key is Valid.
```

### Using scope and metadata parameters
```js
const userId = 'Your UserID';
const licenseKey = 'Your LicenseKey';
const scope = "MyApplicationScope";
const metadata = "Trial License";

const licenseGate = new LicenseGate(userId);

const result = await licenseGate.verify(licenseKey, scope, metadata);

if (result === ValidationType.VALID) {
  console.log('The Key is Valid.');
} else {
  console.log(`The Key is invalid. Reason: ${result}`);
}
```
