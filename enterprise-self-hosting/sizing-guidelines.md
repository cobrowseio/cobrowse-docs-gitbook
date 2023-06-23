# Sizing guidelines

The Cobrowse Enterprise server is an efficient set of micro-services deployed via containers which can run on a single VM, or scale to a highly available architecture serving 100,000+ concurrent sessions across millions of end-users and devices.

In determining hardware requirements for self-hosted or on-premise deployments, please use the following numbers as a guide.&#x20;

### Hardware requirements

* We recommend 1 vCPU and 1 GB memory for every 100 concurrent active co-browsing sessions, plus an extra 2 GB memory for system-level operations
* Storage requirement to run the services (excluding session recording) is 20GB total

### Session recording

If session recording is enabled for your account, storage requirements will increase based on the total number and type of sessions which will be stored.&#x20;

You may estimate how many sessions will be stored by multiplying the # of sessions per month \* the desired data retention length in months.

* **10GB** for every **100,000** web co-browsing session recordings
* **10GB** for every **10,000** mobile co-browsing session recordings with an average length of 5 minutes, and minimal video content, approximately 3.33kb/s
* **10GB** for every **1,000** mobile co-browsing session recordings with average length of 5 minutes, and video content playing 100% of the time, approximately 33.33kb/s

### Database sizing

The underlying database used by Cobrowse Enterprise server is MongoDB. The storage requirement will increase based on the total number of co-browsing sessions which are stored.&#x20;

You may estimate how many sessions will be stored by multiplying the # of sessions per month \* the desired data retention length in months.

* **4GB** for every **1,000,000** sessions

If you plan to use MongoDB Atlas, or another service provider which limits disk IOPS by storage capacity, we recommend your database size also meets the following minimums.

* **10GB** or **100 IOPS** for every **2,000,000** device registrations

### Example container sizing: Cobrowse.io SaaS instance

{% hint style="warning" %}
This section is for illustration only. The exact container sizing will depend on your use case and will vary by traffic patterns and the platforms you use (e.g. the proxy is only needed for web sessions)
{% endhint %}

As a rough guide the split for our production environment is:

api: 35%\
sockets: 35%\
proxy: 15%\
recording: 15%

MongoDB is separate, currently around the same resource requirements as the containers.

Redis usage is minimal, so resource requirements are low, e.g. < 1/4 vCPU per node.
