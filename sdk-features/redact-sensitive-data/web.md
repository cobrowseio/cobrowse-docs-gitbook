# Web

We provide two methods to redact elements.

#### **Define redaction in your source code**

Redactions are defined as CSS selectors, passed as an array to the Cobrowse SDK.

```javascript
CobrowseIO.redactedViews = ['.redacted', ...some other selectors...]
```

Selectors can also be scoped by URL or [glob pattern](https://www.npmjs.com/package/glob-to-regexp#usage) by providing an `Object` where the key is the URL or glob pattern and the value is the array of `String` CSS Selectors to redact when viewing that page.

The example below will always redact any element with the `redacted` class but will only redact all input elements when viewing any example.com page.

```javascript
CobrowseIO.redactedViews = [ 
    '.redacted',
    { 'https://example.com/*' : [ 'input' ] }
]
```

#### **Define redaction via the web dashboard**

Adding selectors via the dashboard can be useful if your app is already in production and you need to redact a field retrospectively, either due to a missed redaction entry in the app or changing requirements.

Visit [https://cobrowse.io/dashboard/settings/redaction](https://cobrowse.io/dashboard/settings/redaction) and enter your selectors into the Web redaction / unredaction configuration.

### Private by Default

For highly privacy sensitive application you may sometimes want to redact everything on the screen, then selectively "unredact" only the parts your support agents should be able to see. This is particularly useful on applications that require a higher privacy standard or where only specific sections of the application should be visible to the agent.

To ensure everything is redacted by default add  `body` to the redaction selectors then you can specify elements to be unredacted.

```javascript
CobrowseIO.redactedViews = ['body', '.redacted', ...some other selectors...]
CobrowseIO.unredactedViews = ['.unredacted', ...some other selectors...]
```

### Redaction Playground

To explore and modify redaction in your apps you can use the Redaction Playground.

{% content-ref url="redaction-playground.md" %}
[redaction-playground.md](redaction-playground.md)
{% endcontent-ref %}
