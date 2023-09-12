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

Invoices are stored on the blockchain: data is stored on IPFS, and the document hash is persisted on-chain – [more info on this](https://docs.request.network/learn-request-network/introduction-to-the-request-protocol/storage).

* Invoices created with the production environment are persisted on the Gnosis Chain.
* Invoices created on the sandbox environment are persisted on Goerli.

### **How do Requests and Invoices differ from each other?**

The Request Network protocol is all about creating payment requests. They are stored on-chain: data is stored on IPFS, and the document hash is persisted on-chain.

Invoices are what you will be manipulating with the Request Finance API. Invoices are simply an implementation of requests with a predefined schema for their content. Invoices are used by the Request Finance application as a way to practically represent general invoicing data.

The Request Finance API adds a layer of automation on top of simple requests. Whenever an invoice is created, it is possible to have an email sent to the designated payer. The invoice issuer (payee) can also get notified as soon as the corresponding request has been paid (please [get in touch with us](https://www.request.finance/contact-us) if you need access to this API feature).

Knowing if a request has been paid [is not trivial](https://docs.request.network/learn-request-network/guides/detect-a-payment). But invoices have an additional property: a status; so knowing if the underlying request has been paid is as easy as reading this property. Invoices can also be scheduled to create occurrences at regular intervals. This is useful to manage collaborators' salaries.

To summarize:

| Differences | Request (protocol level)                   | Invoice (API level)                                                   |
| ----------- | ------------------------------------------ | --------------------------------------------------------------------- |
| Schema      | <p>contentData</p><p>can be any object</p> | contentData represents an invoice, the schema is validated by the API |
| Automation  | X                                          | ✔                                                                     |
| Recurrence  | X                                          | ✔                                                                     |

### **How is data encrypted?**&#x20;

With the Request Finance API, everything that is stored on-chain is encrypted on the network (Gnosis Chain + IPFS) and invoices cannot be made public. Access is limited to the counterparties and the Request Finance team.&#x20;

Although we technically have access to invoice data, we will never use or share this data.&#x20;

If end-to-end encryption is paramount for your usage, we recommend you interact directly with the Request Network protocol through the [Request Client](https://docs.request.network/get-started/request-network-client-introduction) instead of using the Request Finance API. You won't be able however to interact with invoices stored on[ app.request.finance](https://app.request.finance).

### **What rate limits do you have on the API?**&#x20;

The following rate limits apply globally:&#x20;

* Unauthenticated users: 500 calls / hour
* Authenticated users: 5000 calls / hour
* Applications using [OAuth authentication](going-live.md): 15000 calls / hour

Additionally, there are some endpoints that have stricter rate limiting. The API always returns the following information in the headers:&#x20;

* `X-Rate-Limit-Limit`: the rate limit ceiling for that given endpoint
* `X-Rate-Limit-Remaining`: the number of requests left for that given endpoint
* `X-Rate-Limit-Reset`: the remaining time window before the rate limit resets, in seconds

We also send a `Retry-After` in the header on blocked requests to let you know when to call then endpoint again.&#x20;

### **Which currencies and networks do you support?**&#x20;

* The list of currencies accepted by our API is here: [https://api.request.finance/currency](https://api.request.finance/currency)
* The network list is available here: [https://api.request.finance/currency/chains](https://api.request.finance/currency/chains)

### **I am integrating the Request Network. Can I get access to users' data on Request Finance?**

Teams integrating Request Network can build Request Finance-based POCs. To do this, you need to know your public key and add a snippet to invite users to share a request with you (see below).&#x20;

<details>

<summary>Public Key</summary>

```javascript
const hdkey = require('ethereumjs-wallet/hdkey');
const Wallet = require('ethereumjs-wallet').default;
const privateKey = hdkey.fromMasterSeed('random')._hdkey._privateKey;
const wallet = Wallet.fromPrivateKey(privateKey);
console.log(wallet.getPublicKeyString());
```

Source: [https://ethereum.stackexchange.com/questions/11253/ethereumjs-how-to-get-public-key-from-private-key](https://ethereum.stackexchange.com/questions/11253/ethereumjs-how-to-get-public-key-from-private-key)

</details>

<details>

<summary>Snippet</summary>

```javascript
const BUILDER_KEY = "026c0594b192ebfda22706bff76ee5fb34a65fe93fde779ade3dfafbf77375cd2e";
const WEBHOOK_URL = "http://localhost:3001/";
window.open(
  `http://baguette-app.request.finance/add-stakeholder?stakeholder-public-key=${BUILDER_KEY}&webhook-url=${WEBHOOK_URL}`,
  null,
  "popup,width=530,height=760,left=100,top=100"
);
```

</details>

#### Webhook

Once the transaction is persisted, a POST query is sent to the webhook URL. The JSON payload contains the requestId. It only works if the user keeps the page open during the process.

In case of an error, the call is attempted 2 more times.

#### UX

Here's what it looks like to the end user of your application:

![](<.gitbook/assets/Adding a stakeholder - Step 1.png>)<img src=".gitbook/assets/Adding a stakeholder - Step 2 (1).png" alt="" data-size="original">
