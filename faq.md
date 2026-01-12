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

In our tests in `rocky:9 docker` configured the permissions even with setting the `:z` flag, please see the [docker documentation about bind mounts](https://docs.docker.com/engine/storage/bind-mounts/#configure-the-selinux-label) for more details.

#### Caddy as reverse proxy provisioning TLS certificates

In an environment with `SELINUX=enforcing` where Caddy is used as reverse proxy, it was observed that it is necessary to run
```bash
setsebool -P httpd_can_network_connect on
```
to enable Caddy to connect to the internet in order to provision proper TLS certificates. 

## Errors

### 413 RequestEntityTooLarge

When An Otter Wiki raises the error 413 RequestEntityTooLarge please configure the variable `MAX_FORM_MEMORY_SIZE` which is in bytes and by default `500000`, see [Configuration](/Configuration#content-and-editing-preferences).

### Listen queue size is greater than the system max net.core.somaxconn

This was reported to happen when using the `-slim` image on a Synology NAS, see [#342](https://github.com/redimp/otterwiki/issues/342). This can be fixed with increasing the value via sysctl or when using a `docker-compose.yaml` with

```yaml
services:
  otterwiki:
    image: redimp/otterwiki:2-slim
    restart: unless-stopped
    ports:
      - 8080:80
    volumes:
      - ./app-data:/app-data
    sysctls:
      - net.core.somaxconn=1024
```
