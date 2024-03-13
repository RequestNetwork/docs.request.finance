# Going Live

## Prerequisite

Before going live, make sure you have read the page about [organizations.md](organizations.md "mention"). It's important to handle the case where your users' data resides in an organization vs. their personal account.

## Authentication

Replace API keys with the more secure OAuth authentication to go live with your application. Please follow these steps:

1. Your integration needs to be whitelisted by the Request Finance team; get in touch [here](mailto:support@request.finance) or via the Intercom chat. Be sure to mention the email of the account that will call the API. We advise setting up an account just for the integration, an account that does not send or receive invoices, with a strong password that can be archived safely.
2.  Create an app in your Request Finance account on the ["Developers" menu](https://app.request.finance/developers/apps).

    <figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
3. Retrieve the access and refresh tokens from the provided Client Secret and Client ID:
   1. If you act on your users' behalf (ex: you are reading your users' invoices), follow the [Authorization Code Flow](https://auth0.com/docs/get-started/authentication-and-authorization-flow/authorization-code-flow). You can use any OAuth2 or OpenID-compatible library to do the heavy lifting. Auth0 also provides its [own libraries here](https://auth0.com/docs/quickstart/webapp); choose any package corresponding to your language under "Regular Web App." For reference, we developed two implementation examples, one using NextJS and one using ExpressJS, available on this [demo repository](https://github.com/RequestNetwork/demo-api).
   2. If you act on your account's behalf (ex: you are sending invoices from your account to external users), you will need to follow the [Client Credentials Flow](https://auth0.com/docs/get-started/authentication-and-authorization-flow/client-credentials-flow). Reach out to us first to activate this flow. Note that you can only use one type of OAuth flow per app. You can find an example of how to implement it [here](https://auth0.com/docs/get-started/authentication-and-authorization-flow/client-credentials-flow/call-your-api-using-the-client-credentials-flow#example-post-to-token-url). This flow only contains an access token (no refresh token), so you don't need to use the `offline_access` scope in the next step.
4.  To configure the OAuth connection, you need to use the following parameters:

    ```
    URL: https://auth.request.finance
    Audience: accounts
    Scope: openid profile email offline_access
    ```

    Other scopes are also available, like `user:read`, see [#list-organizations-the-user-belongs-to](organizations.md#list-organizations-the-user-belongs-to "mention").
5. Save the access token and, in case of Authorization Code Flow, the refresh token too. Authenticate your API calls using the access token and reuse it until it expires (24 hours). After it expires, use the refresh token to ask for a new access token, or, in the case of Client Credential Flow, use your app secret to ask for a new access token. Here is an example of how to use the access token with NodeJS. Notice the mandatory `X-Network` header. Its values are either `test` or `live`.

{% code fullWidth="false" %}
```json
fetch("https://api.request.finance/invoices", {
    headers: {
      Authorization: `Bearer ${accessToken}`,
      "X-Network": "live",
    },
  })
```
{% endcode %}

## Optional: Transactional Emails

By default, we don't send transactional notifications for invoices created via API. If your users create invoices through your integration, you probably want issuers to be alerted of new invoices, and other transactional emails to be sent. In such case, please get in touch [here](mailto:support@request.finance) or via the Intercom chat, and share your Client ID with the Request Finance team.

## Troubleshooting

### User does not have the right scope

If you are missing a scope for a given user (this can be verified by checking the issued `access_token` on [https://jwt.io](https://jwt.io)), you can add `prompt=consent` to the authorization URL to force updating the allowed scopes.

For instance, if you cannot get a `refresh_token` for a specific user, that user might miss the `offline_access` scope.
