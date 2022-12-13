---
title: Encryption with the API
sidebar_label: Encryption with the API
keywords:
  - Request
  - encryption
  - API
description: Learn how to integrate the Request Finance API and its features.
---

# About Encryption

By default, anything you store on the Request Network protocol can be read by anyone. That might be what you are looking for or not!

To give you control over this, the Request Network protocol supports end-to-end encryption. It means that no one—outside of a request stakeholder (usually, its payer and payee)—can read its information.

Request Finance API, however, **does not** offer end-to-end encryption but **does** allow you to remove public access. What it means is that your invoices are stored encrypted on the network (Gnosis Chain + IPFS) but we, at Request Finance, have access to invoices data.

:::info Although we technically have access to invoices data, we will never use or share this data. We plan to withdraw our access to any encrypted data to provide end-to-end encryption with the API while keeping the best possible experience for our builders.

If end-to-end encryption is paramount for your usage, we recommend you interact directly with the Request Network protocol through the [Request Client](https://docs.request.network/docs/guides/5-request-client/0-intro) instead of using the Request Finance API. You won't be able however to interact with invoices stored on [app.request.finance](https://app.request.finance). :::
