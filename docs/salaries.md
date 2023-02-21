# Payroll

Request Finance allows you to pay net salaries, bonuses, or token vestings to your employees with a couple of clicks. With the API, you can create salary payments from your own tool and pay them later via the Request Finance application.

On a technical level, a payroll payment is just another type of [invoice](invoices.md). As with invoices, payments also need to be converted to an on-chain request after they are created.

## Creating an Off-Chain Payroll Payment

Creating an off-chain payroll payment is done via the same [endpoint that is used to create invoices](invoices.md#creating-an-off-chain-invoice), but with some changes to the attributes (see below).

Note that while the endpoint uses the same attributes, they're handled differently in the Request Finance application. Notably:&#x20;

* `meta`: `format` must be `rnf_salary` to ensure that the salary payment shows up as such in the Request Finance application.
* A `sellerInfo` object is included that includes information about the employee to be paid.
* Some information can be omitted. Refer to the example request below.

{% swagger method="post" path="/invoices" baseUrl="https://api.request.finance" summary="Creates an off-chain payroll payment" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="meta" type="Request Network JSON Schema" %}
Format of the underlying request. Refer to the [Request Network protocol documentation](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/data-format#available-json-schema) for all possible values.\
\
Use\
`{`

`format: "rnf_salary",`

`version: "0.0.3",`

`}`

To create a payroll payment
{% endswagger-parameter %}

{% swagger-parameter in="body" name="creationDate" type="String" %}
ISO-8601 representation of the payment’s creation date.

\




\


Default value:

\


Current date
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceItems.currency" required="true" type="String" %}
Currency code in which the payroll payment is denominated. For example, payments can be denominated in USD, but the employee can be paid in crypto.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceItems.name" type="String" %}
Name of the salary, bonus (e.g. Salary - February 2023).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceItems.quantity" type="Decimal" required="true" %}
Quantity. Use 

`1`

 for payroll payments.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceItems.unitPrice" type="Integer" required="true" %}
Payroll amount.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceNumber" type="String" required="true" %}
Payment number. Has to be unique for each payroll payment.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sellerInfo.email" type="String" required="true" %}
Email of the employee who receives the payment.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sellerInfo.firstName" type="String" %}
First name (incl. middle names) of the employee.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sellerInfo.lastName" type="String" %}
Last name of the employee.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="paymentTerms.dueDate" required="false" type="String" %}
ISO-8601 due date of the payroll payment.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="paymentAddress" type="String" required="true" %}
Address which will receive the payment.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="paymentCurrency" required="true" type="String" %}
Currency in which the payroll payment can be paid. Please review our 

[Currency API](https://api.request.finance/currency/list/invoicing)

 for a list of available currencies.
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="Off-chain payment successfully created" %}
```json
{
    "id": "63f313638c008a5de553ca0d",
    "paymentCurrency": "USDC-matic",
    "sellerInfo": {
        "email": "william.dean@abc-unicorn.com",
        "firstName": "William",
        "lastName": "Dean",
        "userId": "63c4eeed02a7c1bb872cb2a1"
    },
    "paymentAddress": "0x4886E85E192cdBC81d42D89256a81dAb990CDD74",
    "invoiceItems": [
        {
            "tax": {
                "type": "fixed",
                "amount": "0"
            },
            "currency": "USD",
            "quantity": 1,
            "unitPrice": "100000",
            "name": "Salary"
        }
    ],
    "paymentTerms": {
        "dueDate": "2022-12-28T21:59:59.999Z"
    },
    "invoiceNumber": "1",
    "creationDate": "2022-12-22T14:38:16.916Z",
    "meta": {
        "format": "rnf_salary",
        "version": "0.0.3"
    },
    "buyerInfo": {
        "email": "oliver.wilson@abc-unicorn.com",
        "address": {
            "streetAddress": "4933 Birchwood Road",
            "extendedAddress": "",
            "city": "London",
            "postalCode": "ABC123",
            "region": "London",
            "country": "GB"
            "locality": "",
            "country-name": "",
            "extended-address": "",
            "postal-code": "",
            "street-address": ""
        },
        "businessName": "ABC Unicorn",
        "firstName": "Oliver",
        "lastName": "Wilson",
        "taxRegistration": "",
        "userId": "63882ad2b52be3f3d751f668"
    },
    "createdBy": "63882ad2b52be3f3d751f668",
    "paymentOptions": [
        {
            "type": "wallet",
            "value": {
                "currencies": [
                    "USDC-matic"
                ],
                "paymentInformation": {
                    "paymentAddress": "0x4886E85E192cdBC81d42D89256a81dAb990CDD74",
                    "chain": "matic"
                }
            }
        }
    ],
    "type": "live",
    "role": "buyer"
}
```
{% endswagger-response %}
{% endswagger %}

#### **Example Request**

Let’s assume you want to pay your employee, William Dean (william.dean@abc-unicorn.com), $1,000.00 using USDC on Polygon.

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

As with invoices, in the JSON response, you will get an `id` field. Please save it in a variable or in your database. You will need it to convert the payment into an on-chain request in the next section.

## Converting an Off-Chain Salary Payment into an On-Chain Request

Converting a salary payment to an on-chain request to make it payable is done using the same [endpoint that is used for invoices](invoices.md#converting-an-off-chain-invoice-into-an-on-chain-request). Replace `[id]` with the ID of the payment you saved previously. You don’t need to pass anything in the request body.

## Paying a Salary Payment

The only way to currently pay an on-chain salary payment is via the Request Finance application. Simply log in to [Request Finance](https://app.request.finance/) and navigate to the ["Salaries" menu](https://app.request.finance/salaries) to make payments individually or in batch.

## Fetching a Salary Payment

To check the status of a salary payment and understand if it has been paid, please use the same [endpoint that is used for invoices](invoices.md#fetching-an-invoice). Replace `[id]` with the `requestId` of the Request (recommended), or the `invoiceId`.
