---
description: Javascript SDK for Web
---

# Web

### Installation

Add this javascript snippet to the top of the `<head>` section of your website.

```markup
<script>
    (function(w,t,c,p,s,e){p=new Promise(function(r){w[c]={client:function(){if(!s){
    s=document.createElement(t);s.src='https://js.cobrowse.io/CobrowseIO.js';s.async=1;
    e=document.getElementsByTagName(t)[0];e.parentNode.insertBefore(s,e);s.onload=function()
    {r(w[c]);};}return p;}};});})(window,'script','CobrowseIO');

    CobrowseIO.license = "<put your license key here>";    
    CobrowseIO.client().then(function(){
        CobrowseIO.start();
    });
</script>
```

#### Add your license key

Please register an account and generate your free License Key at [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings).

This will associate sessions from your website with your Cobrowse.io account.

### Try it out

Once you have your Javascript snippet and license key set up, navigate to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) to see your device listed. You can click the "Connect" button to initiate a Cobrowse session!

**Note: Cobrowse needs to be able to access your website, so localhost addresses are not recommenced for testing. To test on a local development version, we recommend using** [**ngrok**](https://ngrok.com/)**.**

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

