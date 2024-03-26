---
description: >-
  Cobrowse SDK features. If you want to customize the interface, here's how to
  to use a custom UI for the 6-digit code screen.
---

# 6-digit code screen

For details on how to generate a 6-digit code, please see this page:

{% content-ref url="../6-digit-codes.md" %}
[6-digit-codes.md](../6-digit-codes.md)
{% endcontent-ref %}

### Example UIs

Here's some examples of how you can use a custom UI to display a 6 digit code:

{% tabs %}
{% tab title="Web" %}
```javascript
// A generic Alert class
function Alert() {
  var container = document.createElement('div');

  function content(title, description){
    return '\
      <div style="background: rgba(50, 50, 50, 0.4); position: fixed; z-index: 2147483647; bottom: 0; top: 0; left: 0; right: 0">\
        <div style="color: #333; font-family:sans-serif; line-height:140%; position:fixed; padding:25px; background:white; border-radius:15px; z-index:2147483647; top:50px; left:50%; width:75%; max-width:350px; transform:translateX(-50%); box-shadow:0px 0px 15px #33333322;">\
          <div style="text-align:center; margin-top:10px; margin-bottom:20px"><b>'+title+'</b></div>\
          <div>'+description+'</div>\
          <div style="float:right; margin-top:40px; color:rgb(0, 122, 255);">\
            <a class="cobrowse-close" style="cursor:pointer; padding:10px;">Close</a>\
          </div>\
        </div>\
      </div>\
    ';
  }

  this.show = function(title, description) {
    return new Promise(function(resolve) {
      // always replace content to remove listeners;
      container.innerHTML = content(title, description);
      container.querySelector('.cobrowse-close').addEventListener('click', function(){ resolve(true); this.hide() }.bind(this));
      if (document.body) document.body.appendChild(container);
    }.bind(this));
  }.bind(this);

  this.hide = function() {
    if (container.parentNode) {
      container.parentNode.removeChild(container);
    }
  }.bind(this);
}

function showSessionCode() {
  // ensure Cobrowse is loaded
  CobrowseIO.client().then(function() {
    // create a code a display it to the user
    CobrowseIO.createSessionCode().then(function(code) {
      new Alert().show('Support Code', 'Your Support Code is '+code);
    });
  });
}

// Important: you should not create session codes until your user needs
// them. They will expire shortly after creation so do not create them when
// the page loads. Instead we provide a button the user can click to generate
// a new code.
var button = document.createElement('a');
button.innerHTML = '<b>Create a Support Code</b>';
document.body.appendChild(button);
button.addEventListener('click', showSessionCode);

```
{% endtab %}
{% endtabs %}

See this in action: [https://github.com/cobrowseio/cobrowse-sdk-js-examples#standalone-support-code-example](https://github.com/cobrowseio/cobrowse-sdk-js-examples#standalone-support-code-example).
