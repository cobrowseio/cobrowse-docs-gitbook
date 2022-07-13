---
description: Troubleshooting your self-hosted deployment.
---

# Troubleshooting

## Initial deployment

If issues arise during the initial deployment of your self-hosted Cobrowse Enterprise, it is often related to the pods/containers not being able to start, or attempting to start but failing to start up successfully.&#x20;

Often, there is an underlying configuration issue, such as specifying an invalid MongoDB URL. Other times, a corporate proxy might be blocking access to the Docker image repository.&#x20;

Here are the high-level steps to help diagnose the problem:

1. Determine which services have started successfully, and which have failed.
2. Review the configuration of any failed services. Cross-check your configuration with our documentation.&#x20;
3. Analyze the container logs, starting with the failed services if logs are available, and then the running services. The error messages should provide helpful hints on what has gone wrong.&#x20;
4. If the issues persist, please see below!

### Requesting support

Please provide the following information to [hello@cobrowse.io](mailto:hello@cobrowse.io):

1. Which type of deployment are you using? (i.e. AWS, GCP, Azure, Helm, Docker-compose, Openshift, etc)
2. Which version of Cobrowse Enterprise and/or service containers are you using?
3. How long has the issue occurred? Has it ever worked? If so, has anything changed on your side?&#x20;
4. Are all the services up and running? Please share with us the current status of the services.&#x20;
5. Please send us the container logs for the problematic services. If you are not sure, please share the container logs for all services.
