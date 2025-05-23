---
description: Have greater control over networking requests made by the mobile SDKs.
---

# Intercepting mobile SDK network requests

{% tabs %}
{% tab title="iOS" %}
You can provide the `URLSession` that will be used for all network requests made by the Cobrowse SDK.

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
Cobrowse SDK uses [OkHttp](https://github.com/square/okhttp) for network communication. You can provide an `OkHttpClient` instance that will be used for all HTTP requests made by the Cobrowse SDK.

```kotlin
CobrowseIO.instance().okHttpClient(
    OkHttpClient.Builder()
        .addInterceptor(LoggingInterceptor())
        .build()
)
```

By setting an interceptor on the provided `OkHttpClient` object it will be told of any network request. You can then take action such as throwing an exception to block the network request from being made.

```kotlin
class LoggingInterceptor : Interceptor {
    companion object {
        private val allowedHosts = setOf(".cobrowse.io")
    }

    override fun intercept(chain: Interceptor.Chain): Response {
        val request = chain.request()
        val host = request.url().host()
        if (allowedHosts.any { host.endsWith(it) }) {
            return chain.proceed(request)
        } else {
            throw IOException("This request is not allowed: " + request.url())
        }
    }
}
```
{% endtab %}
{% endtabs %}
