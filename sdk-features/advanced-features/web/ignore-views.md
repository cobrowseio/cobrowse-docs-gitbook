# Ignore Views

It may be useful to ignore specific views from being presented to the agents. To achieve this, you'll need to add our javascript snippet and configure the set of selectors that should be ignored:

```javascript
CobrowseIO.ignoredViews = ['.ignore-view'];
CobrowseIO.start();
```

**With this option, any view that matches any of the configured selectors is removed from the agent view.**

{% hint style="info" %}
Ignored views are different from [redacted views](../../redact-sensitive-data.md), which appear black to the agent.
{% endhint %}
