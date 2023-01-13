# Declaring capabilities

Several of our capabilities, like full device or remote control, can be enabled or disabled from the Session Settings of the dashboard. However, you may wish to have greater control allowing for capabilities to be enabled for some use cases but disabled for another.

{% tabs %}
{% tab title="Web" %}
```
CobrowseIO.capabilities = [ 'cursor', ... some other capabilities ];
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
`CobrowseIO.capabilities = [ ... ];` must be called before `CobrowseIO.start();`
{% endhint %}

Any capability included in the array will be enabled. To disable a capability omit it from the array.

A full list of capabilities can be seen in the table below.

| Capability   | Description                                                         |
| ------------ | ------------------------------------------------------------------- |
| cursor       | Should agent cursor be rendered on client side                      |
| drawing      | Can the agent draw over the users screen                            |
| full\_device | Is full device mode enabled                                         |
| keypress     | Can the agent generate key events                                   |
| laser        | Can the agent direct user with laser pointer                        |
| pointer      | Can the agent point and click on things when in remote control mode |
| scroll       | Can the agent scroll the page (web only)                            |
| select       | Can the agent select text (web only)                                |

{% embed url="https://cobrowse.io/demo" %}

#### Examples

Allow the agent to scroll but not enter any values or navigate

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.capabilities = [ 'cursor', 'drawing', 'full_device', 'laser', 'scroll' ];
```
{% endtab %}
{% endtabs %}

Disable full device mode

{% tabs %}
{% tab title="Web" %}
```
CobrowseIO.capabilities = [ 'cursor', 'drawing', 'keypress', 'laser', 'pointer', 'scroll', 'select' ];
```
{% endtab %}
{% endtabs %}
