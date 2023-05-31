# Getting Started

The Request API is organized around REST. It accepts JSON-encoded bodies, returns JSON-encoded responses, and uses standard HTTP response codes and Bearer authentication.

To get started you need an API key. If you have not already done, go to [Request Finance](https://app.request.finance) and create your account for free. You can find your API keys in the ["Developer" tab](https://app.request.finance/account/api-keys) in “Settings”.

## Authentication

All API endpoints are authenticated. The API supports two modes of authentication:

* Using API keys which are great to start working on your project quickly.
* Using OAuth which is more secure and suitable for production. Learn more about going live [here](going-live.md).

## Base URL

When using the Request Finance API, please use this URL: [https://api.request.finance/](https://api.request.finance/).

## Header

All HTTP requests must include the following headers:

* `Accept: application/json`
* `Content-Type: application/json`
* `Authorization: [YOUR_API_KEY]`

The Authorization header is used to authenticate yourself. Please replace `[YOUR_API_KEY]` with your API key.
