# IE 11 polyfills

The Cobrowse.io JS SDK works well with IE 11, but in order to keep the SDK small there's some polyfills we don't include by default. You may add these to your website yourself to support IE11. The ones we use are:

```markup
<script src="//cdnjs.cloudflare.com/ajax/libs/dom4/2.0.0/dom4.js"></script>
<script crossorigin="anonymous" src="https://polyfill.io/v3/polyfill.min.js?features=CustomEvent%2Cfetch%2CPromise"></script>
```

Make sure to add these before the Cobrowse snippet so they're available when the Cobrowse snippet runs.

