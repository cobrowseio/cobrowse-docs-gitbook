# Start using the enterprise instance

Once your self-hosted instance is up and running, there are a few steps to bootstrap the account and configure our SDKs to use your private instance.

### Account setup

Please first setup a superuser account. Once you have a superuser head to [https://cobrowse.example.com/admin](https://cobrowse.example.com/admin). This will list all the accounts that have been created in your instance. To remove the development-only banner, please set the "Period Members" limit to a high value \(using the "X of \[limit\]" edit box\).

### Setting API location in the SDKs

You will need to set the API location when integrating the SDKs in order to point them at your private instance.

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.api = "https://cobrowse.example.com";
```
{% endtab %}

{% tab title="iOS" %}
```objectivec
CobrowseIO.instance.api = @"https://cobrowse.example.com";
```
{% endtab %}

{% tab title="Android" %}
```java
CobrowseIO.instance().api("https://cobrowse.example.com");
```
{% endtab %}
{% endtabs %}

