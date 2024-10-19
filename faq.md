# FAQ

We cover both frequently asked questions and special cases that would polute the documentation here.

## Installation

### Environments with SELinux

To make An Otter Wiki run in an environment with `SELINUX=enforcing` with the methods propose in the [[Installation]] document the bind mounts have to be adjusted. 

When podman gives you the error message
```
mkdir: cannot create directory '/app-data': Permission denied
```
please update your `compose.yaml` to
```yaml
services:
  otterwiki:
    image: redimp/otterwiki:2
    restart: unless-stopped
    ports:
      - 8080:80
    volumes:
      - ./app-data:/app-data:Z
```

For podman please see the [podman troubleshooting guide](https://github.com/containers/podman/blob/main/troubleshooting.md#2-cant-use-volume-mount-get-permission-denied) for more details and instructions. 

In our tests in rocky:9 docker configured the permissions even with setting the `:z` flag, please see the [docker documentation about bind mounts](https://docs.docker.com/engine/storage/bind-mounts/#configure-the-selinux-label) for more details.
