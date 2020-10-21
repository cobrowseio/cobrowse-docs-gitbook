# IFrames support

## Cross-document IFrames

_Note: IFrames that are not cross document are supported out-of-box without adding the SDK to the embedded page. This guide is for supporting cross document IFrames._

To use Cobrowse for Web with **cross document** IFrames you will need to add our javascript snippet to the page being embedded. You will then need to add a list of trusted origins that will be allowed to access the page content for Cobrowse purposes:

```javascript
// NOTE: this configuration should go into the page being embedded, NOT the top level page.
CobrowseIO.trustedOrigins = [
    'https://myexample.com',
    'https://my-other-website.net'
];
CobrowseIO.start();
```

It is **not** necessary for the embedded page to contain the Cobrowse license key, if provided it will be ignored.

## Running only in an IFrame

A few customers want to run Cobrowse.io only within an IFrame, and not any containing or parent page. This is supported, but requires passing an extra configuration option when starting Cobrowse. 

```javascript
CobrowseIO.start({allowIFrameStart:true});
```



