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

`version: "0.0.3"`

`}`

To create a payroll payment
{% endswagger-parameter %}

{% swagger-parameter in="body" name="creationDate" type="String" %}
ISO-8601 representation of the payment’s creation date.\
\
Default value:\
Current date
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceItems.currency" required="true" type="String" %}
Currency code in which the payroll payment is denominated. For example, payments can be denominated in USD, but the employee can be paid in crypto.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceItems.name" type="String" %}
Name of the salary, bonus (e.g. Salary - February 2023).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceItems.quantity" type="Decimal" required="true" %}
Quantity. Use `1` for payroll payments.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceItems.unitPrice" type="Integer" required="true" %}
Payroll amount.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceNumber" type="String" required="true" %}
Payment number. Has to be unique for each payroll payment.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="buyerInfo.email" required="true" %}
Email of the employer.
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
Currency in which the payroll payment can be paid. Please review our [Currency API](https://api.request.finance/currency/list/invoicing) for a list of available currencies.
{% endswagger-parameter %}

{% swagger-parameter in="body" type="String" name="recurringRule" %}
Used to create a recurring payment. Input as defined by the [ical RFC](https://www.rfc-editor.org/rfc/rfc5545). Recommended tool: [https://jakubroztocil.github.io/rrule/](https://jakubroztocil.github.io/rrule/). \
\
Example:&#x20;

DTSTART:20230314T085800Z RRULE:FREQ=MONTHLY;INTERVAL=1\
\
Monthly on the 14th of each month, starting 14th of March 2023.&#x20;
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="Off-chain payment successfully created" %}
```json
{
    "id": "647855dc484f371e737cf593",
    "paymentCurrency": "USDC-matic",
    "buyerInfo": {
        "email": "joe.bloggs@abc-unicorn.com",
        "userId": "647804be4e8042efbde7a725"
    },
    "sellerInfo": {
        "email": "william.dean@abc-unicorn.com",
        "firstName": "William",
        "lastName": "Dean",
        "userId": "63c4eeed02a7c1bb872cb3a1"
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
            "unitPrice": "100002",
            "name": "Salary"
        }
    ],
    "paymentTerms": {
        "dueDate": "2023-06-01T14:59:59.999Z"
    },
    "recurringRule": "DTSTART:20230601T074619Z\nRRULE:FREQ=MONTHLY;INTERVAL=1;COUNT=3",
    "invoiceNumber": "1",
    "creationDate": "2023-05-31T14:59:59.999Z",
    "meta": {
        "format": "rnf_salary",
        "version": "0.0.3"
    },
    "nextOccurrence": "2023-07-01T07:46:19.000Z",
    "status": "scheduled",
    "createdBy": "647804be4e8042efbde7a725",
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
    "miscellaneous": {
        "notifications": {
            "creation": true
        }
    },
    "role": "buyer",
    "tags": [
       "my_tag"
   ]
}
```
{% endswagger-response %}
{% endswagger %}

#### **Example Request**

Let’s assume you want to pay your employee, William Dean (william.dean@abc-unicorn.com), $1,000.00 using USDC on Polygon. Additionally, the salary should be created every month for the next three months.

To create a salary payment like this, you would pass the following body with the request:

<pre class="language-json"><code class="lang-json"><strong>{
</strong>  "meta": {
    "format": "rnf_salary",
    "version": "0.0.3"
  },
  "creationDate": "2023-05-31T14:59:59.999Z",
  "invoiceItems": [
    {
      "currency": "USD",
      "name": "Salary",
      "quantity": 1,
      "unitPrice": "100002"
    }
  ],
  "invoiceNumber": "1",
  "buyerInfo": {
    "email": "joe.bloggs@abc-unicorn.com"
  },
  "sellerInfo": {
    "email": "william.dean@abc-unicorn.com",
    "firstName": "William",
    "lastName": "Dean"
  },
  "paymentTerms": {
    "dueDate": "2023-06-01T14:59:59.999Z"
  },
  "paymentAddress": "0x4886E85E192cdBC81d42D89256a81dAb990CDD74",
  "paymentCurrency": "USDC-matic",
  "recurringRule": "DTSTART:20230601T074619Z\nRRULE:FREQ=MONTHLY;INTERVAL=1;COUNT=3"
}
</code></pre>

As with invoices, in the JSON response, you will get an `id` field. Please save it in a variable or in your database. You will need it to convert the payment into an on-chain request in the next section.

## Converting an Off-Chain Salary Payment into an On-Chain Request

Converting a salary payment to an on-chain request to make it payable is done using the same [endpoint that is used for invoices](invoices.md#converting-an-off-chain-invoice-into-an-on-chain-request). Replace `[id]` with the ID of the payment you saved previously. You don’t need to pass anything in the request body.

## Paying a Salary Payment

The only way to currently pay an on-chain salary payment is via the Request Finance application. Simply log in to [Request Finance](https://app.request.finance/) and navigate to the ["Salaries" menu](https://app.request.finance/salaries) to make payments individually or in batch.

## Fetching a Salary Payment

To check the status of a salary payment and understand if it has been paid, please use the same [endpoint that is used for invoices](invoices.md#fetching-an-invoice). Replace `[id]` with the `requestId` of the Request (recommended), or the `invoiceId`.

<details>

<summary>Example Response</summary>

```json
{
    "id": "647855dc484f371e737cf593",
    "paymentCurrency": "USDC-matic",
    "buyerInfo": {
        "email": "joe.bloggs@abc-unicorn.com",
        "userId": "647804be4e8042efbde7a735"
    },
    "sellerInfo": {
        "email": "william.dean@abc-unicorn.com",
        "firstName": "William",
        "lastName": "Dean",
        "userId": "63c4eeed02a7c1bb872cb3a1"
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
            "unitPrice": "100002",
            "name": "Salary"
        }
    ],
    "paymentTerms": {
        "dueDate": "2023-06-01T14:59:59.999Z"
    },
    "recurringRule": "DTSTART:20230601T074619Z\nRRULE:FREQ=MONTHLY;INTERVAL=1;COUNT=3",
    "invoiceNumber": "1",
    "creationDate": "2023-05-31T14:59:59.999Z",
    "meta": {
        "format": "rnf_salary",
        "version": "0.0.3"
    },
    "nextOccurrence": "2023-07-01T07:46:19.000Z",
    "status": "scheduled",
    "createdBy": "647804be4e8042efbde7a735",
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
    "miscellaneous": {
        "builderId": null,
        "createdWith": null,
        "logoUrl": null,
        "variant": null,
        "notifications": {
            "creation": true
        }
    },
    "role": "buyer",
    "tags": [],
    "events": []
}
```

</details>

## Listing Salary Payments

To list salaries, use [LIST invoices](invoices.md#list-invoices). Make sure to filter for salaries payments in your request; \
`invoices?variant=rnf_salary`.
