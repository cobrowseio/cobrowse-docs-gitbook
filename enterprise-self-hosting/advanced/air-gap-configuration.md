# Air gap configuration

Cobrowse Enterprise can be installed with an air gap configuration, isolated from public internet and untrusted local networks.

This document contains the extra steps you will (and won't!) have to take to ensure your installation works in an isolated network.

### Docker images

Cobrowse is distributed using docker container images. You will need to pull these images onto a machine and make them available within the isolated network. There are some options available to accomplish this:

* Use a [self-hosted docker registry](https://docs.docker.com/registry/deploying/) and use `docker push` to populate its images
* Use a [docker save/load strategy](https://stackoverflow.com/questions/23935141/how-to-copy-docker-images-from-one-host-to-another-without-using-a-repository)

By default, the Cobrowse Enterprise helm chart references repository `ghcr.io/cobrowseio` to fetch images. After making your images available internally, you can override the `image.repo` helm value to change the repository images are pulled from. For example:

```yaml
# Fetch containers from repo docker.internal (e.g., 
# "docker.internal/cobrowse-api-enterprise:1.2.3")
image:
  repo: "docker.internal"
  
# Or, fetch containers from host machine (e.g., "cobrowse-api-enterprise:1.2.3")
image:
  repo: ""
```

### Database storage

Cobrowse requires a MongoDB-compatible database for storing its data. You will need to install a MongoDB (or compatible, such as AWS DocumentDB) database accessible from your isolated network.

### Enterprise license checking

Cobrowse Enterprise **does not** require an internet connection to validate your enterprise license, therefore there are no steps needed in an isolated environment.

