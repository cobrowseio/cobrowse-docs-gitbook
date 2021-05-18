# Remote control consent dialog

By default, Cobrowse will show a remote control consent dialog when an agent requests remote control. You may modify and customize the remote control consent prompt as you wish, using the SDK hooks described below.

Admin users may also disable this remote control consent prompt from your account settings if you prefer to remove it entirely: [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings)

Here are the SDK hooks:

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.confirmRemoteControl = function() {
    return new Promise(function(resolve, reject) {
        // replace the code below with your own confirmation prompt
        // calling resolve or reject as a appropriate
        // NOTE: do not use window.confirm in production!
        if (window.confirm('Allow remote control by ' + CobrowseIO.currentSession.agent().name + '?')) 
            resolve(true);
        else reject();
    });
};

```

### Sample UI

We've created a Sample UI that you may drop directly into your website to show customize the remote control consent prompt, which inherits the styles and colors from your website.

```javascript
// A generic consent dialog class
function Consent() {
  var container = document.createElement('div');
  function content(title, description){
    return '\
      <div style="background: rgba(50, 50, 50, 0.4); position: fixed; z-index: 2147483647; bottom: 0; top: 0; left: 0; right: 0">\
        <div style="color: #333; font-family:sans-serif; line-height:140%; position:fixed; padding:25px; background:white; border-radius:15px; z-index:2147483647; top:50px; left:50%; width:75%; max-width:350px; transform:translateX(-50%); box-shadow:0px 0px 15px #33333322;">\
          <div style="text-align:center; margin-top:10px; margin-bottom:20px"><b>'+title+'</b></div>\
          <div>'+description+'</div>\
          <div style="float:right; margin-top:40px; color:rgb(0, 122, 255);">\
            <a class="cobrowse-deny" style="cursor:pointer; padding:10px;">Deny</a>\
            <a class="cobrowse-allow" style="cursor:pointer; padding:10px; font-weight: bold;">Allow</a>\
          </div>\
        </div>\
      </div>\
    ';
  }

  this.show = function(title, description) {
    return new Promise(function(resolve) {
      container.innerHTML = content(title, description);
      container.querySelector('.cobrowse-allow').addEventListener('click', function(){ resolve(true); this.hide() }.bind(this));
      container.querySelector('.cobrowse-deny').addEventListener('click', function(){ resolve(false); this.hide() }.bind(this));
      if (document.body) document.body.appendChild(container);
    }.bind(this));
  }.bind(this);

  this.hide = function() {
    if (container.parentNode) {
      container.parentNode.removeChild(container);
    }
  }.bind(this);
}

// Integration with Cobrowse
CobrowseIO.confirmRemoteControl = function() {
    return new Consent().show('Remote Control Request', 'Do you want to allow remote control by ' + CobrowseIO.currentSession.agent().name + '?');
}
```
{% endtab %}

{% tab title="iOS" %}
Coming soon. 
{% endtab %}

{% tab title="Android" %}
Coming soon.
{% endtab %}
{% endtabs %}

