---
title: Java
layout: default
parent: Wrappers
nav_order: 1
---

<!-- prettier-ignore-start -->
# LicenseGate Wrapper for Java
{: .no_toc }
<!-- prettier-ignore-end -->

<!-- prettier-ignore -->
1. TOC 
{:toc}

## Add as dependency

### Maven

```xml
<repository>
  <id>respark-releases</id>
  <name>Respark - Maven Repository</name>
  <url>https://maven.respark.dev/releases</url>
</repository>

<dependency>
  <groupId>dev.respark.licensegate</groupId>
  <artifactId>license-gate</artifactId>
  <version>1.X.X</version>
</dependency>
```

### Gradle (Groovy)

```groovy
repositories {
  maven {
    url 'https://maven.respark.dev/releases'
  }
}

dependencies {
  implementation 'dev.respark.licensegate:license-gate:1.X.X'
}
```

### Gradle (Kotlin)

```kotlin
repositories {
  maven {
    url = uri("https://maven.respark.dev/releases")
  }
}

dependencies {
  implementation("dev.respark.licensegate:license-gate:1.X.X")
}
```

Replacing 1.X.X with the latest version provided on the [Maven Repository](https://maven.respark.dev/#/releases/dev/respark/licensegate/license-gate/).

### Manual

> Alternatively, you can also download the JAR file from the
> [Maven Repository](https://maven.respark.dev/#/releases/dev/respark/licensegate/license-gate/)
> and add it to your project manually. Or get the `LicenseGate.java` class from
> our
> [GitHub repository](https://github.com/DevLeoko/license-gate-java-wrapper/blob/main/src/main/java/dev/respark/licensegate/LicenseGate.java).
> This class includes all functionality but depends on the _jackson_ library.

## Usage

The wrappers provide a simple way to interact with the LicenseGate API. For
details on the API and its parameters, see the
[API documentation](/api/verify-license). The maven repository also includes the
JavaDocs which provides detailed information about the methods and their
parameters.

### Usage Examples

#### Basic example

```java
// Initialize the LicenseGate client
LicenseGate licenseGate = new LicenseGate("YOUR_USER_ID");

// Check a license
LicenseGate.ValidationType result = licenseGate.verify("LICENSE_KEY");

// Handle the result
if (result == LicenseGate.ValidationType.VALID) {
    // License is valid
} else if (result == LicenseGate.ValidationType.EXPIRED) {
    // License is expired (e.g. prompt user to renew)
} else {
    // License is invalid (check documentation for all possible types)
}
```

#### Supplying your RSA public key for challenge verification

```java
// Your public RSA key (can be found in your settings)
// Ensure to fully include it as is on your settings, do not cut away the "begin" and "end" of it
final String PUBLIC_KEY = "-----BEGIN PUBLIC KEY----- MIIB2d/... -----END PUBLIC KEY-----";

// Initialize the LicenseGate client
LicenseGate licenseGate = new LicenseGate("YOUR_USER_ID", PUBLIC_KEY);
```

#### Compact usage

```java
// In just one line
boolean isValid = new LicenseGate("YOUR_USER_ID")
    .verify("LICENSE_KEY")
    .isValid();
```

#### Passing a scope

```java
boolean isValid = new LicenseGate("YOUR_USER_ID")
    .verify("LICENSE_KEY", "SCOPE")
    .isValid();
```

#### Debugging

```java
// Having trouble? Enable debug mode
new LicenseGate("YOUR_USER_ID").debug().verify("LICENSE_KEY");
```

#### Using your own LicenseGate server

```java
new LicenseGate("YOUR_USER_ID").setValidationServer("https://backend.example.com").verify("LICENSE_KEY");
```
The URL should point to your LicenseGate backend server, not the web interface.
