# Ignore views from the agent view

It may be useful to ignore specific views from being presented to the agents. To achieve this, you'll need to add our javascript snippet and configure the set of selectors that should be ignored:

```javascript
CobrowseIO.ignoredViews = ['.ignore-view'];
CobrowseIO.start();
```

**When this option is set, any view that matches any of the configured selectors will be removed from the agent view.**
