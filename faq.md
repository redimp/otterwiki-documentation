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

### The wiki cannot be served from a subfolder

An Otter Wiki requires a dedicated domain (e.g. `wiki.domain.tld`) and can not be mapped into a subfolder of a domain (e.g. `domain.tld/wiki`). See the requirements in [[Installation|Installation#requirements]].

## Errors

### 413 RequestEntityTooLarge

When An Otter Wiki raises the error 413 RequestEntityTooLarge please configure the variable `MAX_FORM_MEMORY_SIZE` which is in bytes and by default `1000000`, see [Configuration](/Configuration#content-and-editing-preferences).

### Listen queue size is greater than the system max net.core.somaxconn

This was reported to happen when using the `-slim` image on a Synology NAS, see [#342](https://github.com/redimp/otterwiki/issues/342). This can be fixed with increasing the value via sysctl or when using a `docker-compose.yaml` with

```yaml
services:
  otterwiki:
    image: redimp/otterwiki:2-slim
    restart: unless-stopped
    ports:
      - 8080:8080
    volumes:
      - ./app-data:/app-data
    sysctls:
      - net.core.somaxconn=1024
```

### Form submission fails after a tab was left open for a long time

Symptom: saving a page, logging in or submitting any other form fails after the tab had been open for a long time. Cause: the CSRF token expired. CSRF protection (from version **2.20.0**) is controlled by `WTF_CSRF_TIME_LIMIT`, which defaults to `86400` seconds (24 hours). If your users run into this regularly, raise the value, see [[Configuration|Configuration#security]]. Do not work around it by disabling `WTF_CSRF_ENABLED`, that turns off the CSRF protection entirely.

## Administration

### I locked myself out

Symptom: nobody can log into an admin account any more, for example a forgotten admin password, a deleted admin user, or a mail server that is down so the password reset email never arrives. Fix: use the [[command line interface|CLI]], which needs neither a browser nor a working login. Generate a fresh password for your admin user:

```
docker compose exec -u www-data otterwiki flask user password you@example.com --generate
```

The command prints a new password to log in with. If no admin user is left, create one first with `flask user create you@example.com "You" -p admin`. See the [[CLI]] page for running these commands from a source install and for the full command reference.
