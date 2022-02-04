# Authentication (JWTs)

### Overview

JSON Web Tokens (JWTs) can be used for automatic authentication when using the [Agent JS API](agent-sdk/) and/or [IFrame embeds](custom-iframe-embeds.md). For example, adding a JSON Web Token (JWT) as a query parameter allows the IFrame embed to load with the specified user already logged in.&#x20;

{% hint style="success" %}
There's no need to create the specified user ahead of time - this is all done automatically through use of JWTs.
{% endhint %}

### Generate the JWT

The JWT is a token that carries information about which account it is, and who the specified user is. It is cryptographically signed by a RS256 private key on your backend. You will share with us the associated public key in your [account settings](https://cobrowse.io/dashboard/settings/integrations) so that we can verify the request is from you and auto-authenticate the specified user to your account.&#x20;

{% hint style="info" %}
Want to know more about JWTs? See [https://jwt.io/](https://jwt.io) for the standard, open source libraries and more!
{% endhint %}

The JWT you create and sign should contain the following claims:

| Claim           | Description                                                                                                                                                                                                    |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **iat**         | Issued at time - this should be the time you created the JWT.                                                                                                                                                  |
| **exp**         | Expiry time - after this time we won't accept this JWT any more. How long JWTs you create last for is up to you, but we would recommend expiring them no more than a day after creation for security purposes. |
| **aud**         | Audience - must always be `https://cobrowse.io`.                                                                                                                                                               |
| **iss**         | Issuer - Should be the license key for your Cobrowse account which can be found at [account settings](https://cobrowse.io/dashboard/settings).                                                                 |
| **sub**         | Subject - The email of the support agent that you'd like to auto-authenticate. User will be automatically created if it does not yet exist.                                                                    |
| **displayName** | Agent Display Name - The name of the support agent (may be displayed to the end user).                                                                                                                         |
| **policy**      | <p>Policy (optional) - Optionally limit the scope of the JWT (e.g. limiting which devices can be listed and connected to).<br>e.g. <code>{ devices: { user_id: '362545823684324' } }</code></p>                |

{% hint style="warning" %}
Your sub and displayName claims are used for audit trail purposes. Along with the policy claim, they also scope agent access to your end-user's devices/information. This means that setting these claims as shared values between users is not recommended.
{% endhint %}

Follow these steps to generate your JWT:

1. Generate an RS256 key pair by clicking the "Generate" button in the [integration settings page](https://cobrowse.io/dashboard/settings/integrations).
2. Keep the private key safe, and do not change the public key in the JWT SSO text box.&#x20;
3. Sign your claims object using your private key downloaded from step 1. Find a range of JWT signing libraries at [https://jwt.io/](https://jwt.io). (Video guide below.)
4. Add the JWT as a query parameter to the [IFrame source URL](custom-iframe-embeds.md), or pass it to the [Agent JS API](agent-sdk/).

### Step-by-step guidance

We recorded a video showing the complete steps to generate a RS256 private key, use it to sign a JSON object with the required claims, and use it as a query parameter to automatically authenticate the specified user.  Hope it is helpful! [https://www.youtube.com/watch?v=jm8AYUfH9hw](https://www.youtube.com/watch?v=jm8AYUfH9hw)

We also recorded a video showing how to use the JWT Policy claim to restrict the scope of the JWT to a filtered set of devices. [https://www.youtube.com/watch?v=LCNEtzMv5U0](https://www.youtube.com/watch?v=LCNEtzMv5U0)
