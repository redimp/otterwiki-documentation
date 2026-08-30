# Upgrade

This pages describes how to upgrade An Otter Wiki to the latest version. The project  adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html). 

> [!IMPORTANT]
> **Minor version upgrades, e.g. from 2.x to 2.y are effortless.**

If a new version does not work as expected, you can just roll back to the version you previously used. This is safe: the preferences you set through the settings interface live in `/app-data/db.sqlite`, not in the image. Pulling an older tag restores the previous behaviour without losing your configuration.

Check the [CHANGELOG](https://github.com/redimp/otterwiki/blob/main/CHANGELOG.md) for information about fixes and new feature of recent releases.

## Choosing the image variant

An Otter Wiki is published both as the full `redimp/otterwiki:2` image and the lighter `redimp/otterwiki:2-slim` variant, see [[the -slim image variant|Installation#the-slim-image-variant]] for the differences between them. The two are interchangeable, but mind the port when switching: the `-slim` image listens only on port 8080, while the full image listens on both port 80 and port 8080. A port mapping onto the container's port 8080, for example `-p 8080:8080`, therefore works with either variant. A mapping onto port 80, like `-p 8080:80`, works only with the full image, so change it to the container's port 8080 before switching to `-slim`.

## docker cli

Make sure to collect the parameters you've used to create and run the container and start with cleaning up the existing container:

```bash
# stop the running container
docker stop otterwiki
# delete the container
docker rm otterwiki
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

This will pull the latest image and recreate the container if necessary, so you can reduce the downtime to a minimum.

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

When you installed An Otter Wiki in e.g. a virtual environment `venv` update the venv using:
```
./venv/bin/pip install -U .
```

Restart the uwsgi server running the application, in case you are running it as an systemd service run a `systemctl restart otterwiki.service`.
