---
description: >-
  Getting started with Cobrowse. Follow these simple steps to bootstrap your
  account and configure the SDK to use your private instance.
---

# Running your instance

Once your self-hosted instance is up and running, there are a few steps to bootstrap the account and configure our SDKs to use your private instance.

## Register an account

Once your instance is up and running, you will need to register yourself an account. Head to [https://\<your instance domain>/register](https://cobrowse.io/register) to setup your account.&#x20;

You may register multiple accounts in your instance if you would like. Some customers register accounts for dev, test, staging, etc.&#x20;

{% hint style="info" %}
If you are using the Email "magic link" sign-in flow, but do not have a mail server set up and have disabled our default mail provider, then please retrieve the "magic link" from the application logs of the cobrowe-api container by setting the environment variable:

`DEBUG=cbio.email`
{% endhint %}

## Setting API location in the SDKs

You will need to set the API location when integrating the SDKs in order to point them at your private instance.

{% hint style="info" %}
When specifying the API, please do not use a trailing slash.&#x20;
{% endhint %}

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.api = "https://cobrowse.example.com";
```

Insert this line into your Cobrowse initialisation logic. It must be inserted before `CobrowseIO.start()` is called.&#x20;

If using our provided snippet, you can put this before or after the `CobrowseIO.license = "..."` line. &#x20;
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

## Useful resources

Now your instance is up an running, you should review the documentation below to fine tune your deployment.

{% content-ref url="adding-a-superuser.md" %}
[adding-a-superuser.md](adding-a-superuser.md)
{% endcontent-ref %}

{% content-ref url="limiting-account-creation.md" %}
[limiting-account-creation.md](limiting-account-creation.md)
{% endcontent-ref %}

{% content-ref url="configuring-smtp.md" %}
[configuring-smtp.md](configuring-smtp.md)
{% endcontent-ref %}

{% content-ref url="management.md" %}
[management.md](management.md)
{% endcontent-ref %}
