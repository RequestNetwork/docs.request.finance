# Going Live

## Prerequisite

Before going live, make sure you have read the page about [organizations.md](organizations.md "mention"). It's important to handle the case where your users' data resides in an organization vs. their personal account.

## Authentication

To go live with your application, you need to setup the OAuth authentication instead of the API keys, please follow these steps:&#x20;

1. Your integration needs to be whitelisted by the Request Finance team; get in touch [here](mailto:support@request.finance) or via the Intercom chat. Be sure to mention the email of the account that will make the queries. We advise setting up an account just for the integration, an account that does not send or receive invoices, with a strong password that can be archived safely.
2.  Create an app in your Request Finance account on the ["Developers" menu](https://app.request.finance/developers/apps).

    <figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
3. Retrieve an OAuth access-token and refresh-token for your user by following the [Authorization Code Flow](https://auth0.com/docs/get-started/authentication-and-authorization-flow/authorization-code-flow), with the provided Client Secret and Client ID. You can use any OAuth2 or OpenID-compatible library to do the heavy lifting. Auth0 also provides its [own libraries here](https://auth0.com/docs/quickstart/webapp), choose any package corresponding to your language under "Regular Web App". For reference, we developed two implementation examples, one using NextJS and one using ExpressJS, available on this [demo repository](https://github.com/RequestNetwork/demo-api).
4.  To configure the OAuth connection, you need to use the following parameters:

    ```
    URL: https://auth.request.finance
    Audience: accounts
    Scope: openid profile email offline_access
    ```

    Other scopes are also available, like `user:read`, see [#list-organizations-the-user-belongs-to](organizations.md#list-organizations-the-user-belongs-to "mention").
5. Save both the access-token and refresh-token. Authenticate your API calls using the access-token and reuse it until it expires (24 hours). After it expires, use the refresh-token to ask for a new access-token. Here is an example of how to use the access-token with NodeJS:

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

## Optional: Transactional Emails

By default, we don't send transactional notifications for invoices created via API. If your users create invoices through your integration, you probably want issuers to be alerted of new invoices, and other transactional emails to be sent. In such case, please get in touch [here](mailto:support@request.finance) or via the Intercom chat, and share your Client ID with the Request Finance team.&#x20;


## Troubleshooting

### User does not have the right scope

If you are missing a scope for a given user (this can be verified by checking the issued `access_token` in https://jwt.io), you can add `prompt=consent` to the authorization URL to force updating the allowed scopes. 

For instance, if you cannot get a `refresh_token` for a specific user, that user might miss the `offline_access` scope.
