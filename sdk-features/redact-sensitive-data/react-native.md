# React Native

To redact an element in your React Native application you can wrap it in a `Redacted` tag provided by the Cobrowse module:

```javascript
import { Redacted } from 'cobrowse-sdk-react-native';

// ...
<View style={styles.container}>
    <Redacted>
        <Text style={styles.instructions}>This text should be secret</Text>
    </Redacted>
</View>
```

#### **Redact WebView content**

Your app may show web content that contains elements that you wish to redact. This can be achieved by setting the `webviewRedactedViews` property to an array of CSS selectors that identify the elements to be redacted.

```javascript
CobrowseIO.webviewRedactedViews = [ '.redacted', ...some other selectors... ];
```

#### **Redacting views outside React Native**

You can follow the same delegate implementation for [iOS](ios.md#redact-views-within-custom-delegate-via-cobrowseiodelegate) or [Android](android.md#redact-views-within-custom-delegate-via-cobrowseio.redactiondelegate) to identify this views and redact or unredact them by default as required.&#x20;

You can check the provided examples for [iOS](https://github.com/cobrowseio/cobrowse-sdk-react-native/blob/master/Example/ios/Example/AppDelegate.m) and [Android](https://github.com/cobrowseio/cobrowse-sdk-react-native/blob/master/Example/android/app/src/main/java/com/example/MainApplication.java) which redact by default the React Native Dev menu keeping one of the options unredacted to illustrate the technique.

### **Private by Default**

Sometimes you may want to redact everything on the screen, then selectively "unredact" only the parts your support agents should be able to see. This is particularly useful on applications that require a higher privacy standard or where only specific sections of the App should be visible to the agent.

To achieve this, you'll need to follow the delegate implementation and ensure you pass the all your applications root views to the Cobrowse redaction delegate methods for [iOS](ios.md#redact-views-within-custom-delegate-via-cobrowseiodelegate) and [Android](android.md#redact-views-within-custom-delegate-via-cobrowseio.redactiondelegate).

Once you've done this, your application root views will be redacted by default and you'll be able to un-redact child components to make them visible to the agents:

```javascript
import { Unredacted } from 'cobrowse-sdk-react-native';
// ...

<View style={styles.container}>
    <Text>This text should be secret due to the redaction by default implementation</Text>
    <Unredacted>
        <Text>This text will be visible to the agents because it's wrapped with an Unredacted component</Text>
        <Redacted>
            <Text>You can also nest redacted elements inside unredacted ones. This text will be hidden to the agents.</Text>
        </Redacted>
    </Unredacted>
</View>
```

### Redaction Playground

To explore and modify redaction in your apps you can use the Redaction Playground.

{% content-ref url="redaction-playground.md" %}
[redaction-playground.md](redaction-playground.md)
{% endcontent-ref %}
