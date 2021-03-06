# Keda Redis Demo

![Layout of the Demo](Demo.jpg)

This Demo repository shows how to automatically Scale Kubernetes Pods based on the Redis Queue length. The Project can automatically bootstrap the K3d cluster with Flux.
## Cluster Bootstrap

0. Build and push the python docker images to the local k3d repository

```sh
k3d registry create registry.localhost --port 5000
```

```sh
cd demo-apps
docker-compose build && docker-compose push
```

1. Make sure that you have kubectl and the [Flux
   CLI](https://toolkit.fluxcd.io/get-started/#install-the-flux-cli) installed
   locally. Also set your current kubectl context to the cluster you want to
   bootstrap.

2. Prepare a Gitlab-Token that is able to access this repository and has the
   following scopes: `read_api`, `read_repository`, `write_repository`.

3. Choose a path inside this repository for the deployment manifests of the
   cluster. The different clusters are located in `clusters/`. This can either
   be an existing cluster or a new one. Flux will create folders for you if they
   do not exist.

4. Bootstrap the cluster with the information you have prepared in the previous
   steps:

   ```sh
   k3d cluster create --registry-use k3d-registry.localhost:5000
   ```

    ```sh
    GITLAB_TOKEN=<TOKEN> flux bootstrap gitlab \
      --hostname=gitlab.codecentric.de \
      --token-auth \
      --owner=marco \
      --repository=keda-demo \
      --branch=master \
      --path=clusters/<CLUSTER>
    ```

5. If you have bootstrapped a new cluster, add deployment manifests to the
   cluster's folder. You can take a look at existing ones and adjust them for
   the new cluster's needs.
