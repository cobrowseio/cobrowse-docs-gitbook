# Docker proxy configuration

When running our docker-compose setup on a network that requires an HTTP proxy you will need to configure some extra settings. Firstly you will need to familiarise yourself with the docker settings regarding proxies, for this please see the [Docker documentation](https://docs.docker.com/network/proxy/) directly.

Once you have configured the Docker host with your proxy details, you will need to add some extra hostnames that should not be proxied. These hostnames are used internally by the Cobrowse services to contact each other, as such they not externally reachable and cannot be proxied.

Add these hostnames to your `noProxy` configuration:

* `cobrowse-api`
* `cobrowse-api-sockets`
* `cobrowse-frontend`
* `cobrowse-chromium`
* `cobrowse-recording`
* `cobrowse-proxy`
* `redis-node-0`
* `redis-node-1`
* `redis-node-2`
* `mongodb`
* `nginx`

You should ensure that your proxy allows access to at least the following domains:

* Docker hub (`docker.io`, `docker.com`, and all subdomains of those)
* GitHub container registry (`ghcr.io`, `pkg-containers.githubusercontent.com`)

If you are planning to use our built-in support for [LetsEncrypt](https://letsencrypt.org/) to issue your SSL certificates, you will also need to allow access to:

* `acme-v02.api.letsencrypt.org`

{% hint style="info" %}
**Note that this is not an exhaustive list.** Underlying providers may change the domains they require access to without notice. If you are unsure if a domain is required, please provide us with extracts from your proxy software logs showing the domains being accessed and we can assist.
{% endhint %}
