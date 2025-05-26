---
title: Rust
layout: default
parent: Wrappers
nav_order: 3
---

<!-- prettier-ignore-start -->
# LicenseGate Wrapper for Rust
{: .no_toc }
<!-- prettier-ignore-end -->

<!-- prettier-ignore -->
1. TOC 
{:toc}

## About

**This wrapper is not maintained by developers of licensegate, credits go towards [Rohit Sangwan](https://github.com/rohitsangwan01).** 

For detailed information please refer to the original GitHub repository: [rohitsangwan01/licensegate-rs](https://github.com/rohitsangwan01/licensegate-rs)

## Installation

**Crates.io:** [licensegate-rs](https://crates.io/crates/licensegate-rs)   


To install LicenseGate Wrapper, simply add the crate to your `Cargo.toml`:

```toml
[dependencies]
licensegate-rs = "0.1.0"
```

## Quick Start

Here’s a minimal example for verifying a license key:

```rust
use licensegate::{LicenseGate, LicenseGateConfig, ValidationType};

#[tokio::main]
async fn main() {
    let user_id = "ENTER_USER_ID";
    let license_key = "ENTER_LICENSE_KEY";

    let licensegate = LicenseGate::new(user_id);

    match licensegate.verify(LicenseGateConfig::new(license_key)).await {
        Ok(ValidationType::Valid) => println!("The key is valid."),
        Ok(reason) => println!("The key is invalid. Reason: {:?}", reason),
        Err(e) => eprintln!("Connection or server error: {:?}", e),
    }
}
```

## Advanced Usage

### Custom LicenseGate Server URL

```rust
let licensegate = LicenseGate::new(user_id)
    .set_validation_server("https://your.custom.server");
```

### Public RSA Key for Challenge Verification

```rust
let licensegate = LicenseGate::new(user_id)
    .set_public_rsa_key("PUBLIC_RSA_KEY");
```

### License Key with Scoped Access

```rust
let config = LicenseGateConfig::new(license_key)
    .set_scope("pro");
let result = licensegate.verify(config).await;
```