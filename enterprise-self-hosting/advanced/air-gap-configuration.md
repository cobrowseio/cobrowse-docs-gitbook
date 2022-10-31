# Air gap configuration

Cobrowse Enterprise can be installed with an air gap configuration, isolated from public internet and untrusted local networks.

This document contains the extra steps you will have to take to ensure your installation works in an isolated network.

{% hint style="info" %}
**Enterprise license checking**

Cobrowse Enterprise **does not** require an internet connection to validate your enterprise license, therefore there are no steps needed in an isolated environment.
{% endhint %}

### Docker images

Cobrowse is distributed using docker container images. You will need to pull these images and make them available within the isolated network. There are some options available to accomplish this:

* Use a [self-hosted docker registry](https://docs.docker.com/registry/deploying/) and use `docker push` to populate its images
* Use a [docker save/load strategy](https://stackoverflow.com/questions/23935141/how-to-copy-docker-images-from-one-host-to-another-without-using-a-repository)

By default, the Cobrowse Enterprise helm chart references repository `ghcr.io/cobrowseio` to fetch images. After making your images available internally, you can override the `image.repo` helm value to change the repository images are pulled from. For example:

```yaml
# values.yml

# Fetch containers from repo docker.internal (e.g., 
# "docker.internal/cobrowse-api-enterprise:1.2.3")
image:
  repo: "docker.internal"
  
# Or, fetch containers from host machine (e.g., "cobrowse-api-enterprise:1.2.3")
image:
  repo: ""
```

If your internal docker repository requires credentials, you should manage your own docker pull credentials using kubernetes' standard `dockerconfigjson` functionality.

To manage your own credentials, first ensure you do not set an `imageCredentials.password` helm value in your deployment. Then you should create a standard kubernetes docker registry secret named `cobrowse-docker-cfg` (replace `cobrowse` with your helm release name if you've changed it) with your registry credentials. Instructions on how to do this is using `kubectl` are located in the [kubernetes documentation](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-secret-by-providing-credentials-on-the-command-line). For example:

```bash
kubectl create secret docker-registry cobrowse-docker-cfg --docker-server=<server url> --docker-username=<username> --docker-password=<password> --docker-email=<email>
```

Please consult your docker registry documentation for what values should be used for the docker server, username, password and email address.

### Database storage

Cobrowse requires a MongoDB-compatible database for storing its data. You will need to install a MongoDB (or compatible, such as AWS DocumentDB) database accessible from your isolated network.

