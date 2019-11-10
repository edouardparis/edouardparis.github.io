+++
title = "Chainhack 2019-11-10"
outputs = ["Reveal"]
[reveal_hugo]
highlight_theme = "vs"
custom_theme = "css/slides.css"
+++

# Chainhack project: A simple lightning Faucet using LNURL

A little application using Opennode API written in Rust.

<small>Edouard Paris - https://edouard.paris/slides/2019-11-10-chainhack4</small>

---

{{% section %}}

## Lightning Network

* 2013 Payment channels - Nakamoto & Core devs
* 2015 Feb *Lightning Network* - J.Poon - T.Dryja
* 2015 Nov *Reaching the ground with LN* - R. Russell
* 2017 Dec Specs V1.0 Rc | first tx on mainnet
* 2028 Avr Majors implementations are on mainnet

---

![bidirectional-channel](bidirectional-channel.svg)

---

# HTLC

*Hash Time Locked Contract*

* "I will give you X if you give me the preimage of H"
* H = **Payment Hash**, R = **Payment Preimage**
* **Time Locked**: If you don't give me R, I can get my money back after
    delay

Payment = **Amount** + **Payment Hash** + **Delay**

---

![payreq](htlc-forwarding-payreq.svg)


{{% /section %}}

---

![bitcoin_stack](bitcoin_stack.svg)

---

![lnurl_logo](https://raw.githubusercontent.com/btcontract/lnurl-rfc/master/media/logo/logo_600.png)

specs: https://github.com/btcontract/lnurl-rfc

---

![lnurl-withdraw](lnurl-withdraw.svg)

---

{{< slide id="" background-iframe="https://ln-testnet-faucet.cleverapps.io/faucet">}}

---

Written with Rust 🦀 v1.40.0-nightly 🎉

```toml
bech32 = "0.7.1"
hyper = "0.13.0-alpha.4"
dotenv = "0.9.0"
env_logger = "0.7.0"
futures-util = "0.3.1"
image = "0.22.3"
lazy_static = "1.4.0"
lightning-invoice = "0.2.0"
log = "0.4.8"
lnurl = "0.1.0"
opennode = "1.0.0"
qrcode = "0.11.0"
reqwest = { version= "0.10.0-alpha.1", features = ["json"]}
regex = "1.3.1"
serde = { version = "1.0.93", features = ["derive"]}
serde_json = "1.0.41"
tokio = "0.2.0-alpha.6"
tokio-executor = "=0.2.0-alpha.6"
tokio-io = "=0.2.0-alpha.6"
tokio-sync = "=0.2.0-alpha.6"
```

---
Code: https://github.com
