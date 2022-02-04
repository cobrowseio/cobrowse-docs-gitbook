# Full device consent dialog

Before a session is upgraded to [full device](../full-device-capabilities/), some platforms require a consent from the user. Here is how to customize these consent prompts where possible:

{% tabs %}
{% tab title="Web" %}
### Customizing the full device UI on Web

```javascript
CobrowseIO.confirmFullDevice = function() {
    return new Promise(function(resolve, reject) {
        // your logic here
        // 
        // call CobrowseIO.currentSession.fullDevice() 
        // to get current full device state
        // 
        // you must show a button or UI as part of the DOM 
        // for the user to click before resolving the promise
        // 
        // call resolve(true) to start full device, or reject() 
        // to turn off full device
    });
}
```

If your custom confirmation prompt is not working in some browsers, please ensure you resolve the promise in response to a user action, such as a button click. This is required by both Safari and Firefox browsers when calling the browser's getDisplayMedia() API, which is used for full device screen share on Web.  The call to getDisplayMedia() must be made from code which is running in response to a user action, such as in an event handler.
{% endtab %}

{% tab title="macOS" %}
Full device by default, use the [user consent dialog](user-consent-dialog.md).
{% endtab %}

{% tab title="Windows" %}
Full device by default, use the [user consent dialog](user-consent-dialog.md).
{% endtab %}
{% endtabs %}

