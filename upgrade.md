# Upgrade

This pages describes how to upgrade An Otter Wiki to the latest version. The project  adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html). 

> [!IMPORTANT]
> **Minor version upgrades, e.g. from 2.x to 2.y are effortless.**

If a new version does not work as expected, you can just roll back to the version you previously used.

Check the [CHANGELOG](https://github.com/redimp/otterwiki/blob/main/CHANGELOG.md) for information about fixes and new feature of recent releases.

## docker cli

Make sure to collect the parameters you've used to create and run the container and start with cleaning up the existing container:

```bash
# stop the running container
docker stop otterwiki
# delete the container
docker rm wiki
```

Next step is fetching the new image via:

```bash
# pull the new image
docker pull redimp/otterwiki:2
```

and recreate the container, make sure to use the parameters you have used before, e.g.

```bash
# create and run the new container using the latest image
docker run --name otterwiki \
    -p 8080:80 \
    -v $PWD/app-data:/app-data \
    redimp/otterwiki:2
```

## docker compose

To upgrade An Otter Wiki deployed via `docker compose`, find the folder where your `docker-compose.yaml` or `compose.yaml` lives and run

```bash
docker compose pull && docker compose up -d
```

This will pull the latest image and recreate the container if nececessary, so you can reduce the downtime to a minimum.

## podman and podman-compose

Follow the process describe for `docker` and `docker compose` above.

## Kubernetes

When you deployed An Otter Wiki using the official [helm chart](https://github.com/redimp/otterwiki/tree/main/helm) configure the application with `--set image.pullPolicy=Always` and let the deployment do a rollout restart e.g.

```bash
kubectl rollout restart deployment \
    --namespace=NAMESPACE \
    --selector=app.kubernetes.io/name=otterwiki
```

## source version

To check out the latest release run:
```bash
# Update the repository information
git remote update origin
# Get the latest release tag
LATEST_RELEASE=$(git describe --tags $(git rev-list --tags --max-count=1))
# Create a new branch named like the latest release version.
git checkout -b $LATEST_RELEASE $LATEST_RELEASE
```

When you installed An Otter Wiki in e.g. an virtual environemt `venv` update the venv using:
```
./venv/bin/pip install -U .
```

Restart the uwsgi server running the application, in case you are running it as an systemd service run a `systemctl restart otterwiki.service`.
