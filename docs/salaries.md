# Salaries

Request Finance allows you to pay net salaries, bonuses, or token vestings to your employees with a couple of clicks. With the API, you can create salary payments from your own tool and pay them later via the Request Finance application.

On a technical level, a salary payment is just another type of [invoice](invoices.md). As with invoices, salary payments also need to be converted to an on-chain request after they are created.

## Creating an Off-Chain Salary Payment

Creating an off-chain salary payment is done via the same [endpoint that is used to create invoices](invoices.md#creating-an-off-chain-invoice), but with some changes to the attributes (see below).&#x20;

**Request**

`POST https://api.request.finance/invoices`

#### **Attributes**

Refer to the [attributes used to create invoices](invoices.md#attributes). However:&#x20;

* `meta`: `format` must be `rnf_salary` to ensure that the salary payment shows up as such in the Request Finance application.
* `buyerInfo` object must include your information.
* Some information can be omitted. Refer to the example request below.&#x20;

Additionally, we require information about who needs to be paid. This information must be included in the `sellerInfo` object.&#x20;

#### **Example Request**

Let’s assume you want to pay your employee, William Dean (william.dean@abc-unicorn.com), $1,000.00 using USDC on Polygon.&#x20;

To create a salary payment like this, you would pass the following body with the request:

```json
{
  "meta": {
    "format": "rnf_salary",
    "version": "0.0.3"
  },
  "creationDate": "2022-12-22T14:38:16.916Z",
  "invoiceItems": [
    {
      "currency": "USD",
      "name": "Salary",
      "quantity": 1,
      "unitPrice": "100000"
    }
  ],
  "invoiceNumber": "1",
  "buyerInfo": {
    "businessName": "ABC Unicorn Ltd.",
    "address": {
      "streetAddress": "4933 Birchwood Road",
      "extendedAddress": "",
      "city": "London",
      "postalCode": "ABC123",
      "region": "London",
      "country": "GB"
    },
    "email": "oliver.wilson@abc-unicorn.com",
    "firstName": "Oliver",
    "lastName": "Wilson"
  },
  "sellerInfo": {
    "email": "william.dean@abc-unicorn.com",
    "firstName": "William",
    "lastName": "Dean"
  },
  "paymentTerms": {
    "dueDate": "2022-12-28T21:59:59.999Z"
  },
  "paymentAddress": "0x4886E85E192cdBC81d42D89256a81dAb990CDD74",
  "paymentCurrency": "USDC-matic"
}
```

As with invoices, in the JSON response, you will get an `id` field. Please save it in a variable or in your database. You will need it to convert the payment into an on-chain request in the next section.&#x20;

## Converting an Off-Chain Salary Payment into an On-Chain Request

Converting a salary payment to an on-chain request to make it payable is done using the same [endpoint that is used for invoices](invoices.md#converting-an-off-chain-invoice-into-an-on-chain-request). Replace `[id]` with the ID of the payment you saved previously. You don’t need to pass anything in the request body.&#x20;

## Fetching a Salary Payment

To check the status of a salary payment and understand if it has been paid, please use the same [endpoint that is used for invoices](invoices.md#fetching-an-invoice). Replace `[id]` with the `requestId` of the Request (recommended), or the `invoiceId`.

## Paying a Salary Payment

The only way to currently pay an on-chain salary payment is via the Request Finance application. Simply log in to [Request Finance](https://app.request.finance/) and navigate to the ["Salaries" menu](https://app.request.finance/salaries) to make payments individually or in batch.
