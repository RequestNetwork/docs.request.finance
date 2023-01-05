# FAQ

### **What is the relation between the Request Finance API and the Request Network?**

The Request Network is a [decentralized protocol](https://github.com/RequestNetwork/requestNetwork) built on Ethereum for creating payment requests. It is maintained by the Request Network Foundation. Request Finance is a commercial (centralized) application that interacts with the Request Network protocol. It is a product of the Request Labs company.\
The Request Finance API is the backend of the Request Finance application. It is an abstraction over the Request Network protocol for easy but centralized access. It's the fastest way to integrate Request into your product.

### **What are the benefits and disadvantages of using the Request Finance API vs. the Request Network?**

**Request Finance API - Benefits**

* Simple REST API, familiar to web developers
* No key management
* Faster response time
* Notifications on new requests and received payments
* Sends invoices to users based on their email instead of their ETH address
* Easy to build applications on top of the Request Network with OAuth
* The easy choice for encrypted data on the network

**Request Finance API - Disadvantages**

* Not fully decentralized
* Users need a Request Finance account to connect

### **Where are invoices stored?**&#x20;

Invoices are stored on the blockchain: data is stored on IPFS, and the document hash is persisted on-chain – [more info on this](https://docs.request.network/docs/guides/1-protocol/4-storage).

* Invoices created with the production environment are persisted on the Gnosis Chain.
* Invoices created on the sandbox environment are persisted on Goerli.

### **How do Requests and Invoices differ from each other?**

The Request Network protocol is all about creating payment requests. They are stored on-chain: data is stored on IPFS, and the document hash is persisted on-chain.

Invoices are what you will be manipulating with the Request Finance API. Invoices are simply an implementation of requests with a predefined schema for their content. Invoices are used by the Request Finance application as a way to practically represent general invoicing data.

The Request Finance API adds a layer of automation on top of simple requests. Whenever an invoice is created, it is possible to have an email sent to the designated payer. The invoice issuer (payee) can also get notified as soon as the corresponding request has been paid (please [get in touch with us](https://www.request.finance/contact-us) if you need access to this API feature).

Knowing if a request has been paid [is not trivial](https://docs.request.network/docs/guides/2-request-client/1-payment-networks). But invoices have an additional property: a status; so knowing if the underlying request has been paid is as easy as reading this property. Invoices can also be scheduled to create occurrences at regular intervals. This is useful to manage collaborators' salaries.

To summarize:

| Differences | Request (protocol level)                   | Invoice (API level)                                                   |
| ----------- | ------------------------------------------ | --------------------------------------------------------------------- |
| Schema      | <p>contentData</p><p>can be any object</p> | contentData represents an invoice, the schema is validated by the API |
| Automation  | X                                          | ✔                                                                     |
| Recurrence  | X                                          | ✔                                                                     |

### **How is data encrypted?**&#x20;

With the Request Finance API, everything that is stored on-chain is encrypted on the network (Gnosis Chain + IPFS) and invoices cannot be made public. Access is limited to the counterparties and the Request Finance team.&#x20;

Although we technically have access to invoice data, we will never use or share this data.&#x20;

If end-to-end encryption is paramount for your usage, we recommend you interact directly with the Request Network protocol through the[ Request Client](https://docs.request.network/docs/guides/2-request-client/0-intro) instead of using the Request Finance API. You won't be able however to interact with invoices stored on[ app.request.finance](https://app.request.finance).\
