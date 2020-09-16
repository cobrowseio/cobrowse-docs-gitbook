# Embed options

We recommend embedding the Cobrowse Dashboard URL \([https://cobrowse.io/dashboard](https://cobrowse.io/dashboard)\) as an IFrame in your CRM. You can pre-filter it as described below to show specific devices in different contexts.

If you are using our 6-digit codes to initiate sessions, and that is the only way you'd like to support, we have a simplified Cobrowse Code URL \([https://cobrowse.io/code](https://cobrowse.io/code)\) that you can embed.

You may also link to or embed the Cobrowse Connect URL \([https://cobrowse.io/connect?filter\_device\_id=123](https://cobrowse.io/connect?filter_device_id=123)\), which will automatically try to connect to the matching device. If multiple devices match the filters you provide, you will be prompted which to select. Make sure to set customData in our SDKs, and use those values in your filters!

We support several query parameters to configure what is shown:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Query Parameter</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>nochrome</code>
      </td>
      <td style="text-align:left">Set to <code>true</code> to remove the navigation bar. For example <code>?nochrome=true</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>token</code>
      </td>
      <td style="text-align:left">Provide a JWT to auto-authenticate the specified user. For example <code>?token=eyJh...dCI6</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>filter_[name]</code>
      </td>
      <td style="text-align:left">
        <p>Filter the list of devices based on any metadata specified in your SDK
          integration. You may add multiple filters as additional query parameters.
          All filters are ANDed together when filtering the device list.</p>
        <p>Please prefix each with &quot;filter_&quot; and ensure the [name] matches
          the field used in your SDK integration.
          <br />For example, <code>?filter_user_email=john@example.com</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>end_action</code>
      </td>
      <td style="text-align:left">
        <p>Set to <code>dashbaord</code> to rediect back to the device list (/dashboard)
          on session end. This is the default behaviour.</p>
        <p>Set to <code>none</code> will show a simple session ended message.</p>
        <p>Set to <code>code</code> will return to the full screen code entry interface
          (/code).</p>
        <p>For example <code>end_action=none</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>allow_popout</code>
      </td>
      <td style="text-align:left">Set to <code>false</code> to disable the popout button when in a session.
        For example<code>?allow_popout=false</code>
      </td>
    </tr>
  </tbody>
</table>

This logic enables you to embed the relevant IFrame data on each place in your CRM. For example, when an agent is looking at a user account view in your CRM, you might use filters to show all of that user's available devices.

