---
description: Have greater control over networking requests made by the mobile SDKs.
---

# Intercepting mobile SDK network requests

{% tabs %}
{% tab title="iOS" %}
You can proved the `URLSession` that will be used for all network requests made by the Cobrowse SDK.

```swift
CobrowseIO.instance().urlSession = \\ Your URLSession object
```

By setting a delegate on the provided `URLSession` object it will be told of any network request. You can then take action such as cancelling the task to block the network request from being made.

```swift
class CustomURLSessionDelegate: NSObject, URLSessionTaskDelegate {
    
    func urlSession(_ session: URLSession, didCreateTask task: URLSessionTask) {
        let allowedHosts = ["api.cobrowse.io"]
        
        guard let host = task.currentRequest?.url?.host, !allowedHosts.contains(host)
            else { return }
        
        print("Blocking data task for host: \(host)")
        task.cancel()
    }
}
```

{% hint style="info" %}
Requires iOS 15+ macOS 12+
{% endhint %}
{% endtab %}

{% tab title="Android" %}

{% endtab %}
{% endtabs %}



