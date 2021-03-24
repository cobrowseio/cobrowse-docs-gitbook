# Sizing guidelines

Cobrowse.io Server is a very efficient set of micro-services built on Docker which can deploy on a single VM, or scale to a highly available architecture serving 100,000+ concurrent sessions across millions of end-users and devices.

In determining hardware requirements for self-hosted or on-premise deployments, please use the following numbers as a guide. 

### Hardware requirements

* We recommend 1 vCPU and 1 GB memory for every 100 concurrent active co-browsing sessions, plus an extra 2 GB memory for system-level operations
* Storage requirement to run the services \(excluding session recording\) is 20GB total

### Session recording

If session recording is enabled for your account, storage requirement will increase based on the total number and type of sessions which will be stored. 

You may estimate how many sessions will be stored by multiplying the \# of sessions per month \* the desired data retention length in months.

* 10GB for every 100,000 web co-browsing session recordings
* 10GB for every 10,000 mobile co-browsing session recordings with an average length of 5 minutes, and minimal video content
* 10GB for every 1,000 mobile co-browsing session recordings with average length of 5 minutes, and video content playing 100% of the time

### Database sizing

The underlying database used by Cobrowse.io Server is MongoDB. The storage requirement will increase based on the total number of co-browsing sessions which are stored. 

You may estimate how many sessions will be stored by multiplying the \# of sessions per month \* the desired data retention length in months.

* 4GB for every 1,000,000 sessions

If you plan to use MongoDB Atlas, or another service provider which limits disk IOPS by storage capacity, we recommend your database size also meets the following minimums.

* 10GB or 100 IOPS for every 2,000,000 device registrations

