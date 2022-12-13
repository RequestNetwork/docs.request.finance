---
title: Portal Introduction
sidebar_label: Portal Introduction
keywords:
  - Request
  - REST
  - API
  - Tutorial
  - Portal
description: Learn how to integrate the Request Finance API and its features.
---

# Introduction

## How to work with Request Finance?

### Front-end

* [https://app.request.finance](https://app.request.finance/) is our production front-end
* [https://baguette-app.request.finance](https://baguette-app.request.finance/) is our staging front-end

We are leveraging Auth0's solution to add authentication and authorization to our applications.

Please use the staging front end whenever you create fake data. You can access the staging front-end with the same account that you use on the production environment or create a specific account as you prefer.

### Storage

Invoices are stored on the blockchain: data is stored on IPFS, and the document hash is persisted on-chain—[more info on this](https://docs.request.network/docs/guides/7-protocol/6-request-ipfs-network).

* Invoices created with the production environment are persisted on the Gnosis Chain.
* Invoices created on the staging environment are persisted on Goerli.

### Payment

When you create an invoice on the staging front end, you can test payments without spending real tokens. We will replace the selected ERC20 payment currency with the FAU token. You can mint some FAU tokens at [https://erc20faucet.com/](https://erc20faucet.com/).

### API

Request Finance API simplifies the use of the Request Network protocol, abstracting all the blockchain complexity. You can create and manage your invoices through a REST API.

Our API accepts JSON-encoded bodies, returns JSON-encoded responses, and uses standard HTTP response codes and Bearer authentication.

Aside from the guide, you can also consult the [OpenAPI specifications](http://redocly.github.io/redoc/?url=https://api.request.network/spec/openapi.yml).

#### Authentication

All API endpoints are authenticated. Two mechanisms are currently allowed:

* API keys, explained below, are great to quickly start working on your project.
* With OAuth, more secure and suitable for production. Please [reach out](https://www.request.finance/contact-us) when you're ready to make the switch.

#### API Keys

If you have not already done it, head towards [Request Finance](https://app.request.finance) and create your account.\
You can find your API keys in the ["developer" section](https://app.request.finance/account/api-keys).

#### API URL

There is only one API URL: [https://api.request.network/](https://api.request.network/) ; but you own two API keys: a "Live" and a "Test" one.

* When using the "Test" API key, invoices will be persisted on Goerli, and you will be able to see them on the staging front-end.
* When using the "Live" API key, invoices will be persisted on Gnosis Chain, and you will be able to see them on the production front end.
