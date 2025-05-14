---
description: >-
  For the times where it's not possible to include the Cobrowse Web SDK into the
  site.
---

# Universal Cobrowse

{% hint style="info" %}
You should have the Web SDK installed on a client web page before following these steps.
{% endhint %}

In some cases your customers using your website might need to complete tasks or view sites that you don't own or can modify. Universal Cobrowse allows for the session to continue to others sites, via our Cobrowse Proxy, whereby the agent can still browse and navigate with your customers in real time.

### Demo

{% embed url="https://vimeo.com/986762574/2fbdc3e837" %}

### Setup

Enable 'Universal Cobrowse' in the Cobrowse dashboard under Settings > General > Session Settings.

You need to supply the SDK with a [glob pattern](https://en.wikipedia.org/wiki/Glob_\(programming\)) of URLs that should launch the Universal Cobrowse experience.

{% hint style="info" %}
You will need to share the domains, including any subdomains, that will be used during a Universal Cobrowse session. Please reach out to your account representative.
{% endhint %}

```
CobrowseIO.universalLinks = [
    'example.com*',
    'example.io/account*,
    '*.net*',
]
```

In the example above when the user clicks on a link that navigates to any of the example URLs below it would show that link within the Universal Cobrowse tabs.

https://example.com\
https://example.com/about\
https://example.io/account?id=123\
https://example.net/help\
https://site.net

If you wish for all links to launch the Universal Cobrowse experience you can use `*` to match all URLs.

```
CobrowseIO.universalLinks = [ '*' ] 
```

You may also use regex. The example below will launch Universal Cobrowse for any link that is not an `example.com` or an `example.io` domain. This can be useful when you might not have a known list of external links.

```
CobrowseIO.universalLinks = [ /^(?!.*(example.com|example.io)).+$/ ]
```

_Notice the regex is an object not a String._

### Redaction within Universal Cobrowse

Redaction is still supported whilst using Universal Cobrowse. Please find full details by using the link below.

{% content-ref url="redact-sensitive-data.md" %}
[redact-sensitive-data.md](redact-sensitive-data.md)
{% endcontent-ref %}

### How does it work?

When using Universal Cobrowse third-party websites are loaded in a modal window within your website. To achieve this experience we proxy all traffic seen in this modal window from the client to our Cobrowse Proxy.

{% hint style="info" %}
Only the pages loaded within the Universal Cobrowse window are proxied to the Cobrowse Proxy. When within a normal Cobrowse session the users requests are not proxied / modified.
{% endhint %}

Any request during the Universal Cobrowse session will be routed and performed through the Cobrowse Proxy, this includes any cookies set on these requests.
