---
description: >-
  Monitor key trends in your session activity and quality from the Cobrowse
  dashboard.
---

# Account metrics

Metrics give you a view of how Cobrowse is being used across your account, such as how many sessions were started, how long they lasted and how well they connected.

You can find them in the dashboard under [Settings > Metrics](https://cobrowse.io/dashboard/settings/metrics).

{% hint style="info" %}
Viewing metrics requires the metrics read permission. The default `administrator` role has this permission.
{% endhint %}

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

### Choosing a time period

Use the date picker at the top of the page to choose the time period to show, for example **Last 4 weeks**.

To see how things have changed, compare against the previous period. Each metric then shows its current value, its previous value and the difference between them.

### Filtering by app and platform

Use the **App** and **Platform** filters to narrow the metrics down to a single app, website or platform. This is useful for spotting a problem that only affects one of your apps, or comparing how sessions perform on web and mobile.

### Charts

Select any metric to show it on the chart below, plotted over the chosen time period. When comparing, the previous period is shown alongside the current one.

Drag across the chart to zoom into a shorter range, and select **Reset zoom** to return to the full period.

### Available metrics

| Metric               | Description                                                                 |
| -------------------- | --------------------------------------------------------------------------- |
| Sessions started     | Number of sessions that became active                                       |
| Agent ratings        | Average rating agents gave their sessions                                   |
| Session duration     | Average length of a session                                                 |
| Declined sessions    | Sessions that were created but never became active                          |
| Device registrations | Number of devices registered with your account                              |
| Time to first frame  | Average time from a session starting until the agent sees the user's screen |
| Round trip time      | Average time for a message to travel from the agent to the device and back  |
| Agent latency        | Average network latency on the agent's side                                 |
| Device latency       | Average network latency on the device's side                                |
| Session restore time | Average time taken to restore a session after its connection was lost       |
| Agent reconnects     | Number of times an agent's connection was lost and restored                 |
| Device reconnects    | Number of times a device's connection was lost and restored                 |
| Session failures     | Sessions that failed to connect, or lost contact with the device            |

### Requesting metrics

The same metrics can be requested via our [Agent SDK](agent-sdk/) and pulled into your own reporting tools.
