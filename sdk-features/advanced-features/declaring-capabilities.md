---
description: >-
  Cobrowse allows for different levels of capabilities which can be enabled or
  disabled from the settings of the dashboard or SDKs.
---

# Declaring capabilities

{% hint style="info" %}
Account-level settings for remote control, full device screenshare, and others are available via your account dashboard. Declaring the supported capabilities on the SDK-side as described below is only necessary for fine-tuning or other advanced use cases.
{% endhint %}

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

<table><thead><tr><th width="214">Capability</th><th>Tool</th><th>Description</th></tr></thead><tbody><tr><td><code>laser</code></td><td><a href="https://support.cobrowse.io/annotation-tools">annotations</a></td><td>Can the agent direct the user with a laser pointer</td></tr><tr><td><code>disappearing_ink</code></td><td><a href="https://support.cobrowse.io/annotation-tools">annotations</a></td><td>Can the agent use the disappearing ink tool<em>(web only)</em></td></tr><tr><td><code>drawing</code></td><td><a href="https://support.cobrowse.io/annotation-tools">annotations</a></td><td>Can the agent draw over the users screen</td></tr><tr><td><code>rectangles</code></td><td><a href="https://support.cobrowse.io/annotation-tools">annotations</a></td><td>Can the agent use the rectangle tool</td></tr><tr><td><code>arrows</code></td><td><a href="https://support.cobrowse.io/annotation-tools">annotations</a></td><td>Can the agent use the arrow tool</td></tr><tr><td><code>full_device</code></td><td><a href="https://support.cobrowse.io/full-device-mode">full device</a></td><td>Is full device mode enabled</td></tr><tr><td><code>cursor</code></td><td>remote control</td><td>Should agent cursor be rendered on client side</td></tr><tr><td><code>keypress</code></td><td>remote control</td><td>Can the agent generate key events</td></tr><tr><td><code>pointer</code></td><td>remote control</td><td>Can the agent point and click on things</td></tr><tr><td><code>scroll</code></td><td>remote control</td><td>Can the agent scroll the page <em>(web only)</em></td></tr><tr><td><code>select</code></td><td>remote control</td><td>Can the agent select text <em>(web only)</em></td></tr><tr><td><code>universal</code></td><td><a href="/sdk-features/universal-cobrowse">universal cobrowse</a></td><td>Is Universal Cobrowse enabled</td></tr></tbody></table>

#### Examples

Allow the agent to scroll but not enter any values or navigate

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.capabilities = [ 'cursor', 'drawing', 'rectangles', 'arrows', 'full_device', 'laser', 'scroll' ];
```
{% endtab %}
{% endtabs %}

Disable full device mode

{% tabs %}
{% tab title="Web" %}
```
CobrowseIO.capabilities = [ 'cursor', 'drawing', 'rectangles', 'arrows', 'keypress', 'laser', 'pointer', 'scroll', 'select' ];
```
{% endtab %}
{% endtabs %}
