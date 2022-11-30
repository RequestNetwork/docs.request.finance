---
title: Invoice API - Introduction
keywords:
  - ERC20
  - DAI
  - Request
  - Invoice
  - Portal
  - API
  - Request Finance
description: Learn how to use Request's invoice API.
---

# Invoice API - Introduction

### Request vs Invoice, How Do They Differ?

Invoices are simply an implementation of requests with a predefined schema for the `contentData` property. Invoices are mainly used by the [Request Finance](https://app.request.finance/) application as a way to represent general invoicing data practically.

Invoice API also differ by the layer of automation added on top of Request API. Whenever an invoice is created, it is possible to have an email sent to the designated payer. The invoice issuer (payee) can also get notified as soon as the corresponding request has been paid (please [contact us](https://www.request.finance/contact-us) if you need more info on this feature).

Knowing if a Request has been paid [is not trivial](2-payment-status.md). But invoices have an additional property: a `status` ; so knowing if the underlying Request has been paid is as easy as reading this property.

Invoices can also be scheduled to create occurrences at regular intervals. This is useful for managing collaborators' salaries.

To summarize:

|                     |                         Request                         |                                       Invoice                                       |
| ------------------- | :-----------------------------------------------------: | :---------------------------------------------------------------------------------: |
| Portal API endpoint |                       `/requests`                       |                                     `/invoices`                                     |
| Schema              | <p><code>contentData</code><br>not validated by API</p> | <p>Invoices extend requests<br><code>contentData</code> schema validated by API</p> |
| Automation          |                            ✖️                           |                                         ✔ ️                                         |
| Recurrence          |                            ✖️                           |                                          ✔                                          |

### Introduction

In this tutorial, we will learn how to use the invoice API to create off-chain invoices and then transform those invoices into on-chain requests.

Please follow the [Portal Introduction](./) to retrieve an API key. Reminder: all HTTP requests must include the following headers:

* `Accept: application/json`
* `Content-Type: application/json`
* `Authorization: [YOUR_API_KEY]`

The Authorization header is used to authenticate yourself. Please replace `[YOUR_API_KEY]` with the previously retrieved key.

### Create a Request With Invoice API

#### Create an Off-Chain Invoice

Use the following endpoint first to create an off-chain invoice that will later be converted to an on-chain Request:

`POST https://api.request.network/invoices`

In the body part, you can use the following example and replace the data accordingly:

```json
{
  "meta": {
    "format": "rnf_invoice",
    "version": "0.0.3"
  },
  // ISO-8601 date
  "creationDate": "2021-09-22T14:38:16.916Z",
  "invoiceItems": [
    {
      "currency": "USD",
      "name": "Television",
      "quantity": 2,
      "tax": {
        "type": "percentage",
        "amount": "20"
      },
      // "unitPrice" is the price of one item excluding taxes
      // Here it means the price of one TV is $99.99
      // The total per item will be $119.99, including taxes, and the whole sum will be $239.98
      "unitPrice": "9999"
    }
  ],
  // "invoiceNumber" has to be unique for each invoice created by your account.
  // It is a string, so it can contain a number, or a more custom invoice identifier.
  "invoiceNumber": "2",
  "buyerInfo": {
    "businessName": "Acme Wholesaler",
    "address": {
      "streetAddress": "4933 Oakwood Avenue",
      "extendedAddress": "",
      "city": "New York",
      "postalCode": "10038",
      "region": "New York",
      // ISO 3166-1 country code
      "country": "US"
    },
    "email": "justin.j-ryan@acme-wholesaler.com",
    "firstName": "Justin",
    "lastName": "J Ryan",
    "taxRegistration": "985-80-3313"
  },
  "sellerInfo": {
    "businessName": "Acme TV Seller",
    "address": {
      "streetAddress": "3521 Park Avenue",
      "extendedAddress": "",
      "city": "HOLTSVILLE",
      "postalCode": "00501",
      "region": "New York",
      // ISO 3166-1 country code
      "country": "US"
    },
    "email": "william.d-hays@acme-tv-seller.com",
    "firstName": "William",
    "lastName": "D Hays",
    "taxRegistration": "965-65-3092"
  },
  "paymentTerms": {
    // ISO-8601 date, last day the buyer can pay
    "dueDate": "2021-10-22T21:59:59.999Z"
  },
  // address which will receive the payment
  "paymentAddress": "0x4886E85E192cdBC81d42D89256a81dAb990CDD74",
  // see https://api.request.network/currency for a list of currencies
  "paymentCurrency": "USDC-matic",
  // optional, you can add several tags to the invoice
  "tags": ["project1", "businessUnit3"]
}
```

In the JSON response, you will get an `id` field. Please save it in a variable or in your database. You will need it in the next section.

#### Convert the Off-Chain Invoice Into an On-Chain Request

Use the following endpoint to convert the previously created off-chain invoice to an on-chain Request:

`POST https://api.request.network/invoices/[id]`

; and replace `[id]` with the previously saved invoice ID.

You don't need to pass anything in the request body this time.

In the JSON response, you will get a `requestId` field. This is the ID of the newly created Request. Please save it in your database, as you will need it to be informed of when the Request has been paid.

#### Know When the Request Has Been Paid

To be informed when the payer has fulfilled the Request, you must poll our API regularly.

Please use the following endpoint to retrieve the status of the Request:

`GET https://api.request.network/invoices/[id]`

Replace `[id]` with the ID of the invoice, or the ID of the Request `requestId`.

You can check the `status` field of the JSON response. The different statuses of an invoice are the following:

* `draft`
* `pending`
* `scheduled`
* `open`
* `accepted`
* `rejected`
* `declaredPaid`
* `paid`
* `canceled`

After creating the Request with the previously described process, you should end up with a `pending` status while the Request is being created on-chain (as this process is asynchronous), followed by an `open` status after the Request has actually been created.

You can use the value `paid` to classify the Request as "fulfilled" and stop polling for a new status.

When the value matches `rejected` or `canceled` you can also stop polling because it means that the Request has been manually cancelled out by the payer (or the payee via its interface) and thus will not get paid.

You should also terminate the polling process if the current date exceeds `paymentTerms.dueDate` value.
