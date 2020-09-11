# JSON Web Tokens \(JWTs\)

Adding a JSON Web Token \(JWT\) as a query parameter allows the IFrame to load with the specified user already logged in. There's no need to create the specified user ahead of time - this is all done automatically through use of JWTs.

The JWT is a token that carries information about which account it is, and who the specified user is. It is cryptographically signed by a RS256 private key on your backend. You will share with us the associated public key so that we can verify the request is from you and auto-authenticate the specified user to your account.HintWant to know more about JWTs? See [https://jwt.io/](https://jwt.io/) for the standard, open source libraries and more!

The JWT you create and sign should contain the following claims:

| Claim | Description | Example |
| :--- | :--- | :--- |
| **iat** | Issued at time - this should be the time you created the JWT. | `1527496190` |
| **exp** | Expiry time - after this time we won't accept this JWT any more. How long JWTs you create last for is up to you, but we would recommend expiring them no more than a day after creation for security puposes. | `1527582590` |
| **aud** | Audience - must always be `https://cobrowse.io`. |  |
| **iss** | Issuer - Should be the License Key for your Cobrowse account which can be found at [settings](https://cobrowse.io/dashboard/settings). | `j6ZCJL61wJ` |
| **sub** | Subject - The email of the support agent that you'd like to auto-authenticate. User will be automatically created if it does not yet exist. | `joe@example.com` |
| **displayName** | Agent Display Name - The name of the support agent \(may be displayed to the end user\). | `Joe Bloggs` |
| **policy** | Policy \(optional\) - Optionally limit the scope of the JWT \(e.g. limiting which devices can be listed and connected to\). |  |

1. Generate a RS256 private key by following the guide [here](https://rietta.com/blog/2012/01/27/openssl-generating-rsa-key-from-command/).
2. Enter the associated public key in your [settings](https://cobrowse.io/dashboard/settings/integrations).
3. Sign your JSON object using your private key.HintFind a range of JWT signing libraries at [https://jwt.io/](https://jwt.io/)
4. Add the JWT as a query parameter to the IFrame source URL in any way you'd like. In our own testing, we set the IFrame source URL to our own backend. Once the request was received by our backend, we generated the JWT, constructed the full URL with the JWT, and did a simple redirect. However, any method will do!

