# Going Live

To go live with your application, you need to setup the OAuth authentication instead of the API keys, please follow these steps:&#x20;

1. Your integration needs to be whitelisted by the Request Finance team; get in touch [here](mailto:support@request.finance) or via the Intercom chat. Be sure to mention the email of the account that will make the queries. We advise setting up an account just for the integration, an account that does not send or receive invoices, with a strong password that can be archived safely.
2.  Create an app in your Request Finance account on the ["Developers" menu](https://app.request.finance/developers/apps).

    <figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
3. Retrieve an OAuth token using the Client Secret and Client ID provided. We suggest using one of Auth0's libraries to handle the heavy lifting: [https://auth0.com/docs/quickstart/webapp](https://auth0.com/docs/quickstart/webapp). Choose the package corresponding to your language under "Regular Web App".
4. Save this token and if possible reuse it until it expires (30 days). Authenticate your API calls using this token. Here is an example of how to use it with NodeJS:

{% code fullWidth="false" %}
```json
fetch("https://api.request.finance/invoices", {
    headers: {
      Authorization: Bearer ${accessToken},
      "X-Network": "live",
    },
  })
```
{% endcode %}

## Optional: transactional emails

By default, we don't send transactional notifications for invoices created via API. If your users create invoices through your integration, you probably want issuers to be alerted of new invoices, and other transactional emails to be sent. In such case, please get in touch [here](mailto:support@request.finance) or via the Intercom chat, and share your Client ID with the Request Finance team.&#x20;
