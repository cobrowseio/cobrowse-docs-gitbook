# Flutter

To redact an element in your Flutter application you can wrap it in a widget provided by the SDK:

```dart
Redacted(child: TextField(/* TextField properties */))
```

{% hint style="info" %}
The widget will be redacted for both client and agent during an active screensharing session. Both client and agent will not be able to interact with it during a session.
{% endhint %}

#### **Redacting views outside Flutter**

You can follow the same delegate implementation for [iOS](ios.md#redact-views-within-custom-delegate-via-cobrowseiodelegate) or [Android](android.md#redact-views-within-custom-delegate-via-cobrowseio.redactiondelegate) to identify this views and redact or unredact them by default as required.&#x20;

### Redaction Playground

To explore and modify redaction in your apps you can use the Redaction Playground.

{% content-ref url="redaction-playground.md" %}
[redaction-playground.md](redaction-playground.md)
{% endcontent-ref %}
