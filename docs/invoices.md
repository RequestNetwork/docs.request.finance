# Invoices

To get paid in cryptocurrency, you first need to create an invoice, which includes details on the buyer, the product or service purchased, the price, taxes, and how you would like to get paid.

After creating the invoice off-chain, it needs to be converted into an on-chain request using another endpoint introduced further below.

{% hint style="info" %}
The Request Network protocol is all about creating payment requests. They are stored on-chain. Invoices are what you will be manipulating with the Request Finance API. Invoices are simply an implementation of requests with a predefined schema for their content. If you wish to learn more about this, please visit [this page](faq.md#how-do-requests-and-invoices-differ-from-each-other).
{% endhint %}

## Creating an Off-Chain Invoice

#### **Request**

`POST https://api.request.finance/invoices`

#### **Attributes**

| Field                             | Description                                                                                                                                                                                                                                                                                                                                                                                       | Format                                        |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| meta                              | <p>Optional. Format of the underlying request. Refer to the <a href="https://github.com/RequestNetwork/requestNetwork/tree/master/packages/data-format#available-json-schema">Request Network protocol documentation</a> for all possible values.<br><br>Default value:<br><code>{</code></p><p><code>format: "rnf_invoice",</code></p><p><code>version: "0.0.3",</code></p><p><code>}</code></p> | <p>Request Network JSON Schema</p><p><br></p> |
| creationDate                      | <p>Optional. ISO-8601 representation of the invoice’s creation date.<br><br>Default value:<br>Current date</p>                                                                                                                                                                                                                                                                                    | String                                        |
| invoiceItems.currency             | <p>Currency code in which the invoice is denominated.<br>For example, invoices can be denominated in USD, but buyers can pay in crypto.</p>                                                                                                                                                                                                                                                       | String                                        |
| invoiceItems.name                 | Optional. Name of the product/service for which the invoice is created.                                                                                                                                                                                                                                                                                                                           | String                                        |
| invoiceItems.quantity             | Quantity of the product/service that was provided.                                                                                                                                                                                                                                                                                                                                                | Decimal                                       |
| invoiceItems.tax.type             | <p>Can be “fixed” or “percentage”. Required if tax.amount is sent.<br><br>Default value:<br><code>fixed</code></p>                                                                                                                                                                                                                                                                                | String                                        |
| invoiceItems.tax.amount           | <p>Amount of the tax. Required if tax.type is sent.<br><br>Default value:<br><code>0</code></p>                                                                                                                                                                                                                                                                                                   | Decimal                                       |
| invoiceItems.unitPrice            | Price of the product/service, excl. taxes.                                                                                                                                                                                                                                                                                                                                                        | Integer                                       |
| invoiceNumber                     | Invoice number. Has to be unique for each invoice.                                                                                                                                                                                                                                                                                                                                                | String                                        |
| buyerInfo.businessName            | Optional. Business name of the buyer (the customer).                                                                                                                                                                                                                                                                                                                                              | String                                        |
| buyerInfo.address.streetAddress   | Optional. Street, house, apartment of the buyer.                                                                                                                                                                                                                                                                                                                                                  | String                                        |
| buyerInfo.address.extendedAddress | Optional. Extended details on the address of the buyer.                                                                                                                                                                                                                                                                                                                                           | String                                        |
| buyerInfo.address.city.postalCode | Optional. Post code of the buyer.                                                                                                                                                                                                                                                                                                                                                                 | String                                        |
| buyerInfo.address.region          | Optional. Region of the buyer (e.g. “California”).                                                                                                                                                                                                                                                                                                                                                | String                                        |
| buyerInfo.address.country         | Optional. Two character ISO 3166-1 country code of the buyer.                                                                                                                                                                                                                                                                                                                                     | String                                        |
| buyerInfo.email                   | Email of the buyer.                                                                                                                                                                                                                                                                                                                                                                               | String                                        |
| buyerInfo.firstName               | Optional. First name (incl. middle names) of the buyer.                                                                                                                                                                                                                                                                                                                                           | String                                        |
| buyerInfo.lastName                | Optional. Last name of the buyer.                                                                                                                                                                                                                                                                                                                                                                 | String                                        |
| buyerInfo.taxRegistration         | Optional. Tax registration number of the buyer.                                                                                                                                                                                                                                                                                                                                                   | String                                        |
| paymentTerms.dueDate              | Optional. ISO-8601 due date of the invoice. Last date the buyer can pay.                                                                                                                                                                                                                                                                                                                          | String                                        |
| paymentAddress                    | Address which will receive the payment.                                                                                                                                                                                                                                                                                                                                                           | String                                        |
| paymentCurrency                   | Currency in which the invoice can be paid. Please review our [Currency API](https://api.request.finance/currency/list/invoicing) for a list of available currencies.                                                                                                                                                                                                                              | String                                        |
| tags                              | Optional. One or multiple tags for an invoice.                                                                                                                                                                                                                                                                                                                                                    | Array of strings                              |

#### **Example Request**

Let’s assume you sold 2 TVs, each worth $99.99, to your customer, Acme Wholesaler Ltd. on the 22nd of December 2022. The tax is 20%, so that’s $119.99 including tax.\
The total invoice amount will thus be $239.98 (=2x $119.99) and it’s payable by the 21st of January 2023.

Your contact at Acme Wholesaler Ltd. is Justin Walton, so you want to send the invoice to his email address, justin.walton@acme-wholesaler.com.

Finally, you want to be paid in USDC on Polygon.

To create an invoice for this, you would pass the following body with the request:

```json
{
   "creationDate": "2022-12-22T14:38:16.916Z",
   "invoiceItems": [
       {
           "currency": "USD",
           "name": "Television",
           "quantity": 2,
           "tax": {
               "type": "percentage",
               "amount": "20"
           },
           "unitPrice": "9999"
       }
   ],
   "invoiceNumber": "13",
   "buyerInfo": {
       "businessName": "Acme Wholesaler Ltd.",
       "address": {
           "streetAddress": "4933 Oakwood Avenue",
           "extendedAddress": "",
           "city": "New York",
           "postalCode": "10038",
           "region": "New York",
           "country": "US"
       },
       "email": "justin.walton@acme-wholesaler.com",
       "firstName": "Justin",
       "lastName": "Walton",
       "taxRegistration": "985-80-3313"
   },
   "paymentTerms": {
       "dueDate": "2023-01-21T23:59:59.999Z"
   },
   "paymentAddress": "0x4886E85E192cdBC81d42D89256a81dAb990CDD74",
   "paymentCurrency": "USDC-matic",
   "tags": [
       "project1",
       "businessUnit3"
   ]
}
```

In the JSON response, you will get an `id` field. Please save it in a variable or in your database. You will need it to convert the invoice into an on-chain request in the next section.

## Converting an Off-Chain Invoice into an On-Chain Request

To make an invoice payable, it must be converted to an on-chain request. Once the invoice is created, convert it into an on-chain request using the endpoint below. Replace `[id]` with the ID of the invoice you saved previously. You don’t need to pass anything in the request body.

#### **Request**

`POST https://api.request.finance/invoices/[id]`

In the JSON response, you will get a `requestId` field. This is the ID of the newly created request. Please save it in your database, as you will need it to be informed of when the request has been paid. You may use this value in place of the `invoiceId` for future HTTP requests.

## Fetching an Invoice

To check the status of an invoice and understand if it has been paid, please poll the endpoint below regularly. Replace `[id]` with the `requestId` of the Request (recommended), or the `invoiceId`.

#### Request

`GET https://api.request.finance/invoices/[id]`

You can check the `status` field of the JSON response. The different statuses of an invoice are the following:

* `open` – The request associated with the invoice has been created on-chain. The buyer has not yet paid the invoice.
* `accepted` – The invoice has been approved by the buyer.
* `declaredPaid` – The buyer declared the invoice as paid. The seller has to confirm before the invoice can move into the paid status. This is necessary for currencies, where the Request Network does not yet support payment detection.
* `paid` – Final state. The buyer paid the invoice.
* `canceled` – Final state. The seller canceled the invoice.
* `rejected` – Final state. The buyer rejected the invoice.
* `scheduled` – Status for recurring invoices. Indicates that an invoice will be created on a specific date in the future.
* `draft` – The invoice is in draft status. It can still be edited and was not yet converted into an on-chain request. This status is currently only supported when creating an invoice in the Request Finance UI.

When creating an on-chain request with the previously described process, you should end up with a pending status while the request is being persisted on-chain (this process is asynchronous), followed by an `open` status once the request is actually created.

You can use the value `paid` to classify the Request as "fulfilled" and stop polling for a new status.

When the value matches `rejected` or `canceled` you can also stop polling: it means that the request has been manually canceled out by the payer or the payee respectively, and thus will not get paid.

You should also terminate the polling process if the current date exceeds `paymentTerms.dueDate`.
