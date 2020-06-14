# Embed options

We recommend embedding the Cobrowse Dashboard URL \([https://cobrowse.io/dashboard](https://cobrowse.io/dashboard)\) as an IFrame in your CRM. You can pre-filter it as described below to show specific devices in different contexts.

If you are using our 6-digit codes to initiate sessions, and that is the only way you'd like to support, we have a simplified Cobrowse Code URL \([https://cobrowse.io/code](https://cobrowse.io/code)\) that you can embed.

You may also link to or embed the Cobrowse Connect URL \([https://cobrowse.io/connect?filter\_device\_id=123](https://cobrowse.io/connect?filter_device_id=123)\), which will automatically try to connect to the matching device. If multiple devices match the filters you provide, you will be prompted which to select. Make sure to set customData in our SDKs, and use those values in your filters!

We support several query parameters to configure what is shown:

| Query Parameter | Description |
| :--- | :--- |
| `nochrome=true` | Removes the navigation bar. |
| `token=[token_value]` | Provide a JWT to auto-authenticate the specified user. For example `?token=eyJh...dCI6` |
| `filter_[name]=[value]` | Filter the list of devices based on any metadata specified in your SDK integration. You may add multiple filters as additional query parameters. All filters are ANDed together when filtering the device list. Please prefix each with "filter\_" and ensure the \[name\] matches the field used in your SDK integration. For example, `?filter_user_email=john@example.com`. |
| `end_action=none` | When session ends, do not return user to /dashboard. You may also use `end_action=code` to return user to /code endpoint. |

This logic enables you to embed the relevant IFrame data on each place in your CRM. For example, when an agent is looking at a user account view in your CRM, you might use filters to show all of that user's available devices.

