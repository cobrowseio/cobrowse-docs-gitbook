# Configuring SMTP

Cobrowse supports configuring a custom SMTP server to replace the default email provider. This will allow you to use your company email to send the magic link login emails.

### Configuring SMTP

1. Firstly make sure you have configured at least one [superuser](adding-a-superuser.md) in your instance. 
2. Open `/admin/configuration` on your instance while logged in as a superuser. e.g. https://example.com/admin/configuration
3. Enter a new configuration key called "**smtp\_url**". The value should be the URL of your SMTP server including any authentiation or non-standard ports.

**Example SMTP URLs**

A standard SMTP URL has the format `smtps://username:password@smtp.example.com`

If your mail server does not require authentication, you can omit the `username:password` fields, e.g.`smtps://smtp.example.com` 

If your mail server does not require a TLS connection, use can use the `smtp://` protocol in the URL instead, e.g. `smtp://smtp.example.com`

If your mail server uses a non-standard port you can specficy that via the URL, e.g. `smtp://smtp.example.com:12345`

