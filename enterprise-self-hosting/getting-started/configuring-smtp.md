# Configuring SMTP

Cobrowse supports configuring a custom SMTP server to replace the default email provider. This will allow you to use your company email to send the magic link login emails.

### Configuring SMTP

1. Firstly make sure you have configured at least one [superuser](adding-a-superuser.md) in your instance.&#x20;
2. Open `/admin/configuration` on your instance while logged in as a superuser. e.g. https://example.com/admin/configuration
3. Enter a new configuration key called "**smtp\_url**". The value should be the URL of your SMTP server including any authentication or non-standard ports.

#### Configure from-address

Please use the username part of the SMTP URL to change the email address seen as the sender of the email. The username should be the full email address you wish to use but it must be an address the SMTP server can send from.

```
smtps://no-reply@example.com@smtp.example.com
```

**Example SMTP URLs**

A standard SMTP URL has the format `smtps://username:password@smtp.example.com`

If your mail server does not require authentication, you can omit the `username:password` fields, e.g.`smtps://smtp.example.com`&#x20;

If your mail server does not require a TLS connection, use can use the `smtp://` protocol in the URL instead, e.g. `smtp://smtp.example.com`

If your mail server uses a non-standard port you can specify that via the URL, e.g. `smtp://smtp.example.com:12345`

#### Troubleshooting

During debugging it can be useful to increase console messages and reduce impacting factors.

{% hint style="info" %}
We only recommend adding these query parameters when debugging in a test environment. We do not recommend adding any of these to your production SMTP URL.
{% endhint %}

#### Additional logging

To increase the log level add the `debug` and `logger` query parameters to your SMTP URL.

`smtps://example.com?debug=true&logger=true`

#### Disable TLS validation

It may be useful to remove any TLS validation during debugging to understand where the problem might be. To do this you can add the `rejectUnauthorized` query parameter with the value of `false` and append port number `25` to the end of the URL.

&#x20;`smtp://example.com:25?tls.rejectUnauthorized=false`
