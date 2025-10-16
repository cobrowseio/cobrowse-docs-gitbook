---
description: Monitor session metrics in realtime to understand session quality.
---

# Session metrics

Whilst in an active Cobrowse session there may be external factors that affect the quality of the session. Most notably varying network connectivity. Session metrics can be used to notify the user of the poor connection allowing for them to make adjustments to improve the connection.

The available metrics are:

<table><thead><tr><th width="146.51953125">Metric</th><th>Description</th></tr></thead><tbody><tr><td>Latency</td><td>Round-trip time taken to respond to a ping from the device to the server.</td></tr><tr><td>Last alive</td><td>The timestamp when the device last successfully pinged the server.</td></tr></tbody></table>

With these it is possible to understand when the device last successfully responded and how long that message took to be acknowledged.

A latencey of **200ms** or below is a good connection.

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.on('session.metrics.updated', session => {
    console.log('Latency:', session.metrics.latency)
    console.log('Last alive:', session.metrics.last_alive)
});
```
{% endtab %}

{% tab title="iOS" %}
```swift
class CobrowseSession: NSObject, CobrowseIODelegate {
    func cobrowseSessionMetricsDidUpdate(_ session: CBIOSession) {
        let metrics = session.metrics()
        
        print("Latency:", metrics.latency())
        print("Last alive:", metrics.lastAlive())
    }
}
```
{% endtab %}

{% tab title="Android" %}
```kotlin
class CobrowseSession : CobrowseIO.SessionMetricsDelegate {
    override fun sessionMetricsDidUpdate(session: Session) {
        val metrics: Session.Metrics = session.metrics()
        
        println("Latency: ${metrics.latency()}")
        println("Last alive: ${metrics.lastAlive()}")
    }
}
```
{% endtab %}
{% endtabs %}
