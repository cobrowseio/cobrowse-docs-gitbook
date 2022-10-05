# Available metrics

Some of the custom Cobrowse instance metrics we provide via our OpenMetrics endpoints are:

| Metric Name                                            | Metric Type | Description                                                                                                       |
| ------------------------------------------------------ | ----------- | ----------------------------------------------------------------------------------------------------------------- |
| **cobrowse\_device\_registrations\_total**             | Counter     | Count of devices registered                                                                                       |
| **cobrowse\_presence\_queue\_delay\_seconds**          | Gauge       | Current wait time for presence queue processing (should be close to 0 most of the time)                           |
| **cobrowse\_presence\_queue\_length\_count**           | Gauge       | Current number of sockets waiting in the presence system processing queue (should be close to 0 most of the time) |
| **cobrowse\_session\_created\_total**                  | Counter     | Count of sessions created                                                                                         |
| **cobrowse\_session\_duration\_seconds**               | Summary     | Duration of sessions                                                                                              |
| **cobrowse\_session\_unused\_total**                   | Counter     | Count of sessions that were created but then never activated                                                      |
| **cobrowse\_websocket\_byte\_in\_total**               | Counter     | Count of WebSocket bytes received                                                                                 |
| **cobrowse\_websocket\_connection\_duration\_seconds** | Summary     | Duration of WebSocket connection                                                                                  |
| **cobrowse\_websocket\_connection\_end\_total**        | Counter     | Count of WebSocket connections ended                                                                              |
| **cobrowse\_websocket\_connection\_start\_total**      | Counter     | Count of WebSocket connections started                                                                            |
| **cobrowse\_websocket\_connections\_count**            | Gauge       | Current number of open WebSocket connections                                                                      |
| **cobrowse\_websocket\_message\_in\_total**            | Counter     | Count of WebSocket messages received                                                                              |
| **cobrowse\_websocket\_message\_size\_bytes**          | Summary     | Size of WebSocket message                                                                                         |

Many of these will depend on the behaviour of your deployment and use case, so we do not provide specific ranges thresholds to monitor by default. You should monitor and decide what the appropriate limits are for your deployment.

As well as these custom metrics, the prometheus client also collects some generic system metrics and NodeJS specific metrics, see below for more information:

{% embed url="https://prometheus.io/docs/instrumenting/writing_clientlibs/#standard-and-runtime-collectors" %}

{% embed url="https://github.com/siimon/prom-client/tree/master/lib/metrics" %}
