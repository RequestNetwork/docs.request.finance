---
description: Learn how to integrate the Request Finance API and its features.
---

# Introduction

Request Finance API simplifies the use of the Request Network protocol, abstracting all the blockchain complexity. You can create and manage your invoices through a REST API.

Our API accepts JSON-encoded bodies, returns JSON-encoded responses, and uses standard HTTP response codes and Bearer authentication.

## How to work with the Request Finance API?

### Authentication

All API endpoints are authenticated. Two mechanisms are currently allowed:

* API keys (explained below) which are great to start working on your project quickly.
* With OAuth, more secure and suitable for production. Please [reach out](https://www.request.finance/contact-us) when you're ready to make the switch.

### API keys

If you have not already done it, head toward [Request Finance](https://app.request.finance) and create your account.\
You can find your API keys in the ["developer" section](https://app.request.finance/account/api-keys).

### API URL

There is only one API URL: [https://api.request.network/](https://api.request.network/) ; but you own two API keys.

* When using the "Test" API key, invoices will be persisted on Goerli, and you will be able to see them on the staging front end.
* When using the "Live" API key, invoices will be persisted on Gnosis Chain, and you will be able to see them on the production front end.

Please use the "Test" API key whenever you create fake data.

### Front-end

* [https://app.request.finance](https://app.request.finance/) is our production front end
* [https://baguette-app.request.finance](https://baguette-app.request.finance/) is our staging front end

You can access the staging front end with the same account that you use on the production environment.

### Storage

Invoices are stored on the blockchain: data is stored on IPFS, and the document hash is persisted on-chain—[more info on this](https://docs.request.network/docs/guides/7-protocol/6-request-ipfs-network).

* Invoices created with the production environment are persisted on the Gnosis Chain.
* Invoices created on the staging environment are persisted on Goerli.

### Payment

When you create an invoice on the staging front end, you can test payments without spending real tokens. We replace the selected ERC20 payment currency with the FAU token. You can mint some FAU tokens at [https://erc20faucet.com/](https://erc20faucet.com/).

### Postman collection

Please visit our [Postman collection](https://www.postman.com/request-finance/workspace/request-finance-api-public/documentation/24913360-b5105a65-a6bd-4247-b3b1-ed60e5c8f5cb) for more details and to try out the API.
