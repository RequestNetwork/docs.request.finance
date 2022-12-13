---
title: Request through the REST API - wrap-up
sidebar_label: REST API wrap-up
description: Learn how to integrate the Request Finance API and its features.
---

# Context

## Request Finance API vs. Request Network protocol

The Request Network is a [decentralized protocol](https://github.com/RequestNetwork/requestNetwork) that adds metadata to payment transactions. It is maintained by the Request Network Foundation.

Request Finance is a commercial (centralized) product built on top of the Request Network protocol. It was created by the Request Labs company.

The Request Finance API is an abstraction over the Request Network protocol for easy but centralized access. It's the fastest way to integrate Request into your product.

### Request Finance API - Pros

* Simple REST API, familiar to web developers
* No key management
* Faster response time
* Notifications on new requests and received payments
* Sends invoices to users based on their email instead of their ETH address.
* Easy to build applications on top of Request with OAuth
* The easy choice for encrypted data on the network

### Request Finance API - Cons

* Not fully decentralized
* Users need a Request Finance account to connect
* Encryption is not end-to-end (see [here](4-api-encryption.md))
