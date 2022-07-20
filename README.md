# okta-pkce-proxy
This project is designed to showcase a simple Cloudflare proxy that permits setting the `access_token` as a cookie that can be consumed by 'legacy' applications.
</br></br>
<table style='border: 1px solid'>
<th>
<span style='color: red;'><b>ALERT!</b></span>
</th>
<tr>
<td>
<p><i>There are security implications when using this model as it is overriding CORS features/capabilities.</i></p>
<p><b>Before utilizing this example in any sort of production environment, be sure to submit it to in-depth security review to ensure it meets your needs.</b></p>
</td>
</tr>
</table>

### okta-auth-js
In order to utilize this proxy, you must either implement some custom methods to overwrite the normal `okta-auth-js` SDK or, at a minimum, implement a custom [`httpRequestClient`](https://github.com/okta/okta-auth-js#httprequestclient) that will send the appropriate `withCredentials: true` argument required by CORS in order to permit the `Set-Cookie` header in the `/token` response.

This example has opted to customize the `exchangeCodeForTokens` and `postToTokenEndpoint` methods as well as implement a custom `httpRequest` client.

### okta-token-proxy
This project depends on [`okta-token-proxy`](https://github.com/eatplaysleep/okta-token-proxy) in order to function. Please ensure you have implemented it as either a Cloudflare worker or some other 'function' that is capable of intercepting the `/token` call from this application.

#### iFrames, Social Auth, & WebAuthn
An additional feature of this project is performing login via a modal using a combination of HTML5 `postMessage` and an iFrame using the [`allow`](https://w3c.github.io/webauthn/#sctn-iframe-guidance) feature policy.

Please be advised of the following limitations of a modal-based login flow:
| | |
| --- | --- |
| **Social Authentication** | You cannot enable social providers in Okta due to nearly all of the standard social providers blocking authorization attempts from an iFrame. |
| **WebAuthn** | *EXPERIMENTAL -- Will only work in Chrome and Edge.* In order to use WebAuthn you **must** set the `REACT_APP_OKTA_AUTH_ALLOW` variable to match the following pattern: <br><br>`publickey-credentials-get https://{your_app_url} https://{your_okta_custom_domain_url} https://{your_okta_tenant}.okta.com`<br><br>Please see [here](https://w3c.github.io/webauthn/#sctn-iframe-guidance) for further implementation details regarding using WebAuthn in an iFrame.

*Use of a custom domain is optional, but highly encouraged. If you do not have one setup, do not include it in the config string.*

## Getting Started
To develop this project locally, copy the `.env.sample` to a `.env` file and provide the necessary values.

Utilize one of the following to start the application:
```
yarn start
```
or
```
npm install && npm start
```
and access the application at

```
http://localhost:3000
```
