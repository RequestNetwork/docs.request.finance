# Invoices

To get paid in cryptocurrency, you first need to create an invoice, which includes details on the buyer, the product or service purchased, the price, taxes, and how you would like to get paid.

After creating the invoice off-chain, it needs to be converted into an on-chain request using another endpoint introduced further below.

{% hint style="info" %}
The Request Network protocol is all about creating payment requests. They are stored on-chain. Invoices are what you will be manipulating with the Request Finance API. Invoices are simply an implementation of requests with a predefined schema for their content. If you wish to learn more about this, please visit [this page](faq.md#how-do-requests-and-invoices-differ-from-each-other).
{% endhint %}

## Creating an Off-Chain Invoice

{% swagger method="post" path="/invoices" baseUrl="https://api.request.finance" summary="Creates an off-chain invoice for the account" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="meta" type="Request Network JSON Schema" %}
Format of the underlying request. Refer to the [Request Network protocol documentation](https://github.com/RequestNetwork/requestNetwork/tree/master/packages/data-format#available-json-schema) for all possible values.\
\
Default value:\
`{`

`format: "rnf_invoice",`

`version: "0.0.3",`

`}`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="creationDate" type="String" %}
ISO-8601 representation of the invoice’s creation date.

\




\


Default value:

\


Current date
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceItems.currency" required="true" type="String" %}
Currency code in which the invoice is denominated. For example, invoices can be denominated in USD, but buyers can pay in crypto.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceItems.name" type="String" %}
Name of the product/service for which the invoice is created.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceItems.quantity" type="Decimal" required="true" %}
Quantity of the product/service that was provided.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceItems.tax.type" type="String" %}
Can be “fixed” or “percentage”. Required if tax.amount is sent.

\




\


Default value:

\




`fixed`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceItems.tax.amount" type="Decimal" %}
Amount of the tax. Required if tax.type is sent.

\




\


Default value:

\




`0`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceItems.unitPrice" type="Integer" required="true" %}
Price of the product/service, excl. taxes.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="invoiceNumber" type="String" required="true" %}
Invoice number. Has to be unique for each invoice.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="buyerInfo.businessName" type="String" %}
Business name of the buyer (the customer).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="buyerInfo.address.streetAddress" type="String" %}
Street, house, apartment of the buyer.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="buyerInfo.address.extendedAddress" type="String" %}
Extended details on the address of the buyer.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="buyerInfo.address.city.postalCode" type="String" %}
Post code of the buyer.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="buyerInfo.address.region" type="String" %}
Region of the buyer (e.g. “California”).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="buyerInfo.address.country" type="String" %}
Two character ISO 3166-1 country code of the buyer.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="buyerInfo.email" type="String" required="true" %}
Email of the buyer.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="buyerInfo.firstName" type="String" %}
First name (incl. middle names) of the buyer.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="buyerInfo.lastName" type="String" %}
Last name of the buyer.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="buyerInfo.taxRegistration" type="String" %}
Tax registration number of the buyer.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="paymentTerms.dueDate" required="false" type="String" %}
ISO-8601 due date of the invoice. Last date the buyer can pay.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="paymentAddress" type="String" required="true" %}
Address which will receive the payment.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="paymentCurrency" required="true" type="String" %}
Currency in which the invoice can be paid. Please review our 

[Currency API](https://api.request.finance/currency/list/invoicing)

 for a list of available currencies.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="tags" type="Array of strings" %}
One or multiple tags for an invoice.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="recurringRule" type="String" %}
Used to create a recurring invoice. Input as defined by the [ical RFC](https://www.rfc-editor.org/rfc/rfc5545). Recommended tool: [https://jakubroztocil.github.io/rrule/](https://jakubroztocil.github.io/rrule/). \
\
Example:&#x20;

DTSTART:20230314T085800Z RRULE:FREQ=MONTHLY;INTERVAL=1\
\
Monthly on the 14th of each month, starting 14th of March 2023.&#x20;
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="Off-chain invoice successfully created" %}
```json
{
    "id": "63f2f5a7f00a45f276585b27",
    "paymentCurrency": "USDC-matic",
    "buyerInfo": {
        "taxRegistration": "985-80-3313",
        "lastName": "Walton",
        "firstName": "Justin",
        "email": "justin.walton@acme-wholesaler.com",
        "businessName": "Acme Wholesaler Ltd.",
        "address": {
            "streetAddress": "4933 Oakwood Avenue",
            "extendedAddress": "",
            "city": "New York",
            "postalCode": "10038",
            "region": "New York",
            "country": "US",
            "locality": "New York",
            "country-name": "US",
            "extended-address": "",
            "postal-code": "10038",
            "street-address": "4933 Oakwood Avenue"
        },
        "userId": "63c8d2c230d75e750fb449ea"
    },
    "paymentAddress": "0x4886E85E192cdBC81d42D89256a81dAb990CDD74",
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
    "paymentTerms": {
        "dueDate": "2023-01-21T23:59:59.999Z"
    },
    "invoiceNumber": "3",
    "creationDate": "2022-12-22T14:38:16.916Z",
    "meta": {
        "format": "rnf_invoice",
        "version": "0.0.3"
    },
    "sellerInfo": {
        "email": "joe.bloggs@acme-seller",
        "address": {
            "streetAddress": "4933 Oakwood Avenue",
            "extendedAddress": "",
            "city": "New York",
            "postalCode": "10038",
            "region": "New York",
            "country": "US",
            "locality": "New York",
            "country-name": "US",
            "extended-address": "",
            "postal-code": "10038",
            "street-address": "4933 Oakwood Avenue"
        },
        "businessName": "Acme Corporation",
        "firstName": "Joe",
        "lastName": "Bloggs",
        "taxRegistration": "123456789",
        "userId": "63995f77f46a063ba7ac34de"
    },
    "createdBy": "63995f77f46a063ba7ac34de",
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
    "role": "seller",
    "tags": [
        "my_tag"
    ]
}
```
{% endswagger-response %}
{% endswagger %}

**Example Request**

Let’s assume you sold 2 TVs, each worth $99.99, to your customer, Acme Wholesaler Ltd. on the 22nd of December 2022. The tax is 20%, so that’s $119.99 including tax.\
The total invoice amount will thus be $239.98 (=2x $119.99) and it’s payable by the 21st of January 2023.

Your contact at Acme Wholesaler Ltd. is Justin Walton, so you want to send the invoice to his email address, justin.walton@acme-wholesaler.com.

Finally, you want to be paid in USDC on Polygon.

To create an invoice for this, you would pass the following body with the request:

<pre class="language-json"><code class="lang-json"><strong>{
</strong>   "creationDate": "2022-12-22T14:38:16.916Z",
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
       "my_tag"
   ]
}
</code></pre>

In the JSON response, you will get an `id` field. Please save it in a variable or your database. In the next section, you will need it to convert the invoice into an on-chain request.

## Converting an Off-Chain Invoice into an On-Chain Request

To make an invoice payable, it must be converted to an on-chain request. Once the invoice is created, convert it into an on-chain request using the endpoint below. Replace `[id]` with the ID of the invoice you saved previously. You don’t need to pass anything in the request body.

{% swagger method="post" path="/invoices/[id]" baseUrl="https://api.request.finance" summary="Converts off-chain invoice into on-chain request" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
ID of the off-chain invoice
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="Request created successfully" %}
```json
{
    "id": "63f2f5a7f00a45f276585b28",
    "paymentCurrency": "USDC-matic",
    "buyerInfo": {
        "taxRegistration": "985-80-3313",
        "lastName": "Walton",
        "firstName": "Justin",
        "email": "justin.walton@acme-wholesaler.com",
        "businessName": "Acme Wholesaler Ltd.",
        "address": {
            "streetAddress": "4933 Oakwood Avenue",
            "extendedAddress": "",
            "city": "New York",
            "postalCode": "10038",
            "region": "New York",
            "country": "US",
            "locality": "New York",
            "country-name": "US",
            "extended-address": "",
            "postal-code": "10038",
            "street-address": "4933 Oakwood Avenue"
        },
        "userId": "63c8d2c230d75e750fb449ea"
    },
    "paymentAddress": "0x4886E85E192cdBC81d42D89256a81dAb990CDD74",
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
    "paymentTerms": {
        "dueDate": "2023-01-21T23:59:59.999Z"
    },
    "invoiceNumber": "3",
    "creationDate": "2022-12-22T14:38:16.916Z",
    "meta": {
        "format": "rnf_invoice",
        "version": "0.0.3"
    },
    "sellerInfo": {
        "email": "joe.bloggs@acme-seller.com",
        "address": {
            "streetAddress": "4933 Oakwood Avenue",
            "extendedAddress": "",
            "city": "New York",
            "postalCode": "10038",
            "region": "New York",
            "country": "US",
            "locality": "New York",
            "country-name": "US",
            "extended-address": "",
            "postal-code": "10038",
            "street-address": "4933 Oakwood Avenue"
        },
        "businessName": "Acme Corporation",
        "firstName": "Joe",
        "lastName": "Bloggs",
        "taxRegistration": "123456789",
        "userId": "63995f77f46a063ba7ac34de"
    },
    "createdBy": "63995f77f46a063ba7ac34de",
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
    "trackedTransactions": [],
    "requestId": "017f7f0b5518c6b3c3711b2f5b4517af10195b0f91dc802c3db36f5bf119595fda",
    "invoiceLinks": {
        "pay": "https://app.request.finance/017f7f0b5518c6b3c3711b2f5b4517af10195b0f91dc802c3db36f5bf119595fda?token=0x68d6728d52aadb034938d908d8adb93bb12e64e8982c40b4af275c260b3aee97&enablePayment=true",
        "view": "https://app.request.finance/017f7f0b5518c6b3c3711b2f5b4517af10195b0f91dc802c3db36f5bf119595fda?token=0x68d6728d52aadb034938d908d8adb93bb12e64e8982c40b4af275c260b3aee97",
        "signUpAndPay": "https://app.request.finance/signup?invoice=017f7f0b5518c6b3c3711b2f5b4517af10195b0f91dc802c3db36f5bf119595fda&token=0x68d6728d52aadb034938d908d8adb93bb12e64e8982c40b4af275c260b3aee97&enablePayment=true"
    },
    "role": "seller",
    "tags": [
        "my_tag"
    ]
}
```
{% endswagger-response %}
{% endswagger %}

In the JSON response, you will get a `requestId` field. This is the ID of the newly created request. Please save it in your database, as you will need it to be informed of when the request has been paid. You may use this value in place of the `invoiceId` for future HTTP requests.

## Sharing your Invoice and Getting Paid

To get paid by your customer, you need to redirect them to a payment page hosted by Request Finance. You can get the links to the payment page from the response after creating an on-chain request and embed them in your application or an email.&#x20;

Using the `invoiceLinks.signUpAndPay` link, you can redirect your customers to a page that looks like the one below, where they can pay your invoice or sign into their Request Finance account, if they have one.&#x20;

When your customers pay through this page using Request Finance, they can enjoy the convenience and security of our platform, which includes measures to prevent accidental double payments. Additionally, this payment method ensures that the invoice status is automatically updated, simplifying invoice tracking for both you and your customers.

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>Sign up &#x26; pay page</p></figcaption></figure>

## &#x20;Fetching an Invoice

To check the status of an invoice and understand if it has been paid, please poll the endpoint below regularly. Replace `[id]` with the `requestId` of the Request (recommended), or the `invoiceId`.

{% swagger method="get" path="/invoices/[id]" baseUrl="https://api.request.finance" summary="Fetch an invoice by its ID" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
ID of the request or invoice
{% endswagger-parameter %}

{% swagger-parameter in="query" name="withLinks=true" type="String" %}
Includes "Share" and "Payment links" in the response. See 

[https://docs.request.finance/invoices#sharing-your-invoice-and-getting-paid](https://docs.request.finance/invoices#sharing-your-invoice-and-getting-paid)


{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    "id": "63f2f5a7f00a45f276585b28",
    "paymentCurrency": "USDC-matic",
    "buyerInfo": {
        "taxRegistration": "985-80-3313",
        "lastName": "Walton",
        "firstName": "Justin",
        "email": "justin.walton@acme-wholesaler.com",
        "businessName": "Acme Wholesaler Ltd.",
        "address": {
            "streetAddress": "4933 Oakwood Avenue",
            "extendedAddress": "",
            "city": "New York",
            "postalCode": "10038",
            "region": "New York",
            "country": "US",
            "locality": "New York",
            "country-name": "US",
            "extended-address": "",
            "postal-code": "10038",
            "street-address": "4933 Oakwood Avenue"
        },
        "userId": "63c8d2c230d75e750fb449ea"
    },
    "paymentAddress": "0x4886E85E192cdBC81d42D89256a81dAb990CDD74",
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
    "paymentTerms": {
        "dueDate": "2023-01-21T23:59:59.999Z"
    },
    "invoiceNumber": "3",
    "creationDate": "2022-12-22T14:38:16.916Z",
    "meta": {
        "format": "rnf_invoice",
        "version": "0.0.3"
    },
    "sellerInfo": {
        "email": "joe.bloggs@acme-seller.com",
        "address": {
            "streetAddress": "4933 Oakwood Avenue",
            "extendedAddress": "",
            "city": "New York",
            "postalCode": "10038",
            "region": "New York",
            "country": "US",
            "locality": "New York",
            "country-name": "US",
            "extended-address": "",
            "postal-code": "10038",
            "street-address": "4933 Oakwood Avenue"
        },
        "businessName": "Acme Corporation",
        "firstName": "Joe",
        "lastName": "Bloggs",
        "taxRegistration": "123456789",
        "userId": "63995f77f46a063ba7ac34de"
    },
    "createdBy": "63995f77f46a063ba7ac34de",
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
    "requestId": "017f7f0b5518c6b3c3711b2f5b4517af10195b0f91dc802c3db36f5bf119595fda",
    "status": "open",
    "paymentMetadata": {},
    "events": [
        {
            "name": "create",
            "userId": "63995f77f46a063ba7ac34de",
            "date": "2023-02-20T04:28:12.000Z"
        }
    ],
    "trackedTransactions": [],
    "role": "seller",
    "tags": [
        "my_tag"
    ]
}
```
{% endswagger-response %}
{% endswagger %}

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

## Listing Invoices

Fetch a list of the user's invoices. Use the `creationDateRange` parameter to filter for a invoices created in a date range. Other filters are listed below for your convenience, for example the `search=tx_hash` filter is a useful filter to use when presenting the user with a list of invoices associated with a transaction hash.&#x20;

{% swagger method="get" path="/invoices" baseUrl="https://api.request.finance" summary="List invoices" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="creationDateRange" type="String" %}
Filter by creation date (ISO 8601 format). 

\


Example: creationDateRange={"from":"2022-06-24T00:00:00.000Z","to":"2022-08-22T00:00:00.000Z"}
{% endswagger-parameter %}

{% swagger-parameter in="query" name="search" type="String" %}
Filter by transaction hash, invoice number, company, and other fields. Example: search=0xef... 
{% endswagger-parameter %}

{% swagger-parameter in="query" name="variant" type="String" %}
Filter by invoice format. Use 

`rnf_invoice`

 to filter for invoices and 

`rnf_salary`

 to filter for salaries. 

\


Example: variant=meta.format
{% endswagger-parameter %}

{% swagger-parameter in="query" type="String" name="withLinks=true" %}
Includes "Share" and "Payment links" in the response. See 

[https://docs.request.finance/invoices#sharing-your-invoice-and-getting-paid](https://docs.request.finance/invoices#sharing-your-invoice-and-getting-paid)


{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
[
    {
        "id": "63f3081d8c008a5de554ca97",
        "paymentCurrency": "USDC-matic",
        "buyerInfo": {
            "taxRegistration": "985-80-3313",
            "lastName": "Walton",
            "firstName": "Justin",
            "email": "justin.walton@acme-wholesaler.com",
            "businessName": "Acme Wholesaler Ltd.",
            "address": {
                "streetAddress": "4933 Oakwood Avenue",
                "extendedAddress": "",
                "city": "New York",
                "postalCode": "10038",
                "region": "New York",
                "country": "US",
                "locality": "New York",
                "country-name": "US",
                "extended-address": "",
                "postal-code": "10038",
                "street-address": "4933 Oakwood Avenue"
            },
            "userId": "63c8d2c230d75e750fb449ea"
        },
        "paymentAddress": "0x4886E85E192cdBC81d42D89256a81dAb990CDD74",
        "invoiceItems": [
            {
                "currency": "USD",
                "name": "TV Stand",
                "quantity": 1,
                "tax": {
                    "type": "percentage",
                    "amount": "20"
                },
                "unitPrice": "2000"
            }
        ],
        "paymentTerms": {
            "dueDate": "2023-01-21T23:59:59.999Z"
        },
        "invoiceNumber": "4",
        "creationDate": "2022-12-22T14:38:16.916Z",
        "meta": {
            "format": "rnf_invoice",
            "version": "0.0.3"
        },
        "sellerInfo": {
            "email": "joe.bloggs@acme-seller.com",
            "address": {
                "streetAddress": "4933 Oakwood Avenue",
                "extendedAddress": "",
                "city": "New York",
                "postalCode": "10038",
                "region": "New York",
                "country": "US",
                "locality": "New York",
                "country-name": "US",
                "extended-address": "",
                "postal-code": "10038",
                "street-address": "4933 Oakwood Avenue"
            },
            "businessName": "Acme Corporation",
            "firstName": "Joe",
            "lastName": "Bloggs",
            "taxRegistration": "123456789",
            "userId": "63995f77f46a063ba7ac34de"
        },
        "createdBy": "63995f77f46a063ba7ac34de",
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
        "requestId": "01cb820a384b1e2c4d746a7e6080530f558bb25a211f1e8e7724ea2b6f9fc2789a",
        "status": "open",
        "role": "seller",
        "tags": [
            "my_tag"
        ],
        "trackedTransactions": []
    },
    {
        "id": "63f2f5a7f00a45f276585b28",
        "paymentCurrency": "USDC-matic",
        "buyerInfo": {
            "taxRegistration": "985-80-3313",
            "lastName": "Walton",
            "firstName": "Justin",
            "email": "justin.walton@acme-wholesaler.com",
            "businessName": "Acme Wholesaler Ltd.",
            "address": {
                "streetAddress": "4933 Oakwood Avenue",
                "extendedAddress": "",
                "city": "New York",
                "postalCode": "10038",
                "region": "New York",
                "country": "US",
                "locality": "New York",
                "country-name": "US",
                "extended-address": "",
                "postal-code": "10038",
                "street-address": "4933 Oakwood Avenue"
            },
            "userId": "63c8d2c230d75e750fb449ea"
        },
        "paymentAddress": "0x4886E85E192cdBC81d42D89256a81dAb990CDD74",
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
        "paymentTerms": {
            "dueDate": "2023-01-21T23:59:59.999Z"
        },
        "invoiceNumber": "3",
        "creationDate": "2022-12-22T14:38:16.916Z",
        "meta": {
            "format": "rnf_invoice",
            "version": "0.0.3"
        },
        "sellerInfo": {
            "email": "joe.bloggs@acme-seller.com",
            "address": {
                "streetAddress": "4933 Oakwood Avenue",
                "extendedAddress": "",
                "city": "New York",
                "postalCode": "10038",
                "region": "New York",
                "country": "US",
                "locality": "New York",
                "country-name": "US",
                "extended-address": "",
                "postal-code": "10038",
                "street-address": "4933 Oakwood Avenue"
            },
            "businessName": "Acme Corporation",
            "firstName": "Joe",
            "lastName": "Bloggs",
            "taxRegistration": "123456789",
            "userId": "63995f77f46a063ba7ac34de"
        },
        "createdBy": "63995f77f46a063ba7ac34de",
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
        "requestId": "017f7f0b5518c6b3c3711b2f5b4517af10195b0f91dc802c3db36f5bf119595fda",
        "status": "open",
        "paymentMetadata": {},
        "events": [
            {
                "name": "create",
                "userId": "63995f77f46a063ba7ac34de",
                "date": "2023-02-20T04:28:12.000Z"
            }
        ],
        "role": "seller",
        "tags": [
            "my_tag"
        ],
        "trackedTransactions": []
    }
]
```
{% endswagger-response %}
{% endswagger %}

