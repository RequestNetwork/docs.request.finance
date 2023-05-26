# Going Live

To go live with your application, please follow these steps:&#x20;

1. You need to be whitelisted by the Request Finance team; get in touch [here](mailto:support@request.finance) or via the Intercom chat.&#x20;
2.  Create an app in your Request Finance account on the ["Developers" menu](https://app.request.finance/developers/apps).

    <figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
3. Authenticate your API calls using the Client Secret and Client ID provided. We suggest using one of Auth0's libraries to handle the heavy lifting: [https://auth0.com/docs/quickstart/webapp](https://auth0.com/docs/quickstart/webapp). Choose the package corresponding to your language under "Regular Web App". Once you get a token, you can use it in API calls like this:&#x20;

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

## Optional: Sending Emails

By default, we are not sending notifications for invoices created via API. If you want us to send automated emails to your clients (for example, to notify them of a new invoice), you need to share your Client ID with the Request Finance team.&#x20;
