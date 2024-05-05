# Installation

The recommend way of running An Otter Wiki is via [[docker compose|Installation#using-docker-compose]]. For deploying in kubernetes via [[Helm|Installation#kubernetes]].

## Requirements

The **CPU** requirements are pretty low, you can run it on a Raspberry Pi 1 A (ARMv6). The **RAM** required is around 100MiB (according to `docker stats`). The required disk **storage** depends on the content, please keep in mind, that the backend is a git repository that never forgets: Deleting a page or an attachment does not free up any space. The wiki needs no internet access. Clients using the wiki need only access to the server running the wiki, so it can run in an environment which is _isolated_ from the internet.

As URLs as dedicated domain (e.g. `wiki.domain.tld`) is required, it can not be mapped into a subfolder.

Client requirement: Javascript must be activated in your browser to use the wiki.

## Using docker cli

An Otter Wiki is published as a Docker image on Docker hub as [`redimp/otterwiki`](https://hub.docker.com/r/redimp/otterwiki). The stable images are build for the plattforms `amd64`, `arm64`, `armv7` and `armv6`.

Make sure you have [docker](https://docs.docker.com/engine/install/) installed.

To run an otter wiki via docker cli, listening on port 8080 and using a local directory for data persistency, use the following command:
```bash
docker run --name otterwiki \
    -p 8080:80 \
    -v $PWD/app-data:/app-data \
    redimp/otterwiki:2
```
Open the wiki via http://127.0.0.1:8080 if you are running the docker command on your machine.

You can configure the application with environment variables e.g.
```
-e SITE_NAME="My Wiki" -e SITE_DESCRIPTION="An otter wiki run via docker"
```
For all configuration options please see [[Configuration]].

## Using docker compose

The recommended way of running An Otter Wiki is via `docker compose`.

1. Create a `docker-compose.yaml` file

    ```yaml
    version: '3'
    services:
      otterwiki:
        image: redimp/otterwiki:2
        restart: unless-stopped
        ports:
          - 8080:80
        volumes:
          - ./app-data:/app-data
    ```

2. Run `docker compose up -d`
3. Access the wiki via http://127.0.0.1:8080 if run on your machine.
4. Register your account. The first account is an admin-account with access to the application settings. The first accounts email address doesn't need to be confirmed nor has the account to be activated.
5. Customize the settings to your liking.
6. It's highly recommended to use a webserver as reverse proxy to connect to  the docker process, see [Reverse Proxy](#reverse-proxy) below.

Alternativly you can configure the application using environment variables, for example:

```yaml
version: '3'
services:
  otterwiki:
    image: redimp/otterwiki:2
    restart: unless-stopped
    ports:
    - 8080:80
    volumes:
    - ./app-data:/app-data
    environemnt:
      MAIL_DEFAULT_SENDER: no-reply@example.com
      MAIL_SERVER: smtp.server.tld
      MAIL_PORT: 465
      MAIL_USERNAME: otterwiki@example.com
      MAIL_PASSWORD: somepassword
      MAIL_USE_SSL: True
```

For all configuration options please see [[Configuration]].

## podman and podman-compose

An Otter Wiki can be run with `podman` and `podman-compose` in the same way as with` docker` and `docker compose` please see above.

## Kubernetes

An Otter Wiki can be conveniently deployed on kubernetes using the official Helm Chart.
For example you can create a new deployment with an ingress on `otterwiki.example.com` with

```bash
helm install otterwiki-example \
  --set config.SITE_DESCRIPTION="An Otter Wiki deployed with Helm" \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host="otterwiki.example.com" \
  oci://registry-1.docker.io/redimp/otterwiki
```

Please refer to the [chart](https://github.com/redimp/otterwiki/tree/main/helm) for
a detailed README with instructions, more examples and informations about the default values
of the chart.

## From source as WSGI application with uwsgi

1. Install the prerequisites

    i. Debian / Ubuntu
    ```
    apt install git build-essential python3-dev python3-venv
    ```
    ii. RHEL8 / Fedora / CentOS8 / Rocky Linux 8
    ```
    yum install make python3-devel
    ```
2. Clone the otterwiki repository and enter the directory
    ```
    git clone https://github.com/redimp/otterwiki.git
    cd otterwiki
    ```
3. Create and initialize the repository where the otterwiki data lives
    ```bash
    mkdir -p app-data/repository
    # initialize the empty repository
    git init app-data/repository
    ```
4. Create a minimal `settings.cfg` e.g. via
    ```bash
    echo "REPOSITORY='${PWD}/app-data/repository'" >> settings.cfg
    echo "SQLALCHEMY_DATABASE_URI='sqlite:///${PWD}/app-data/db.sqlite'" >> settings.cfg
    echo "SECRET_KEY='$(echo $RANDOM | md5sum | head -c 16)'" >> settings.cfg
    ```
5. Create a virtual environment and install An Otter Wiki
    ```
    python3 -m venv venv
    ./venv/bin/pip install -U pip uwsgi
    ./venv/bin/pip install .
    ```
6. Run uwsgi listening on the localhost port 8080
    ```bash
    export OTTERWIKI_SETTINGS=$PWD/settings.cfg
    ./venv/bin/uwsgi --http 127.0.0.1:8080 --master -enable-threads --die-on-term -w otterwiki.server:app
    ```
7. Open http://127.0.0.1:8080 in your browser.
8. Register your account. The first account is an admin-account with access to the application settings.
9. Alternatively can you configure the application using the `settings.cfg`. For all configuration options please see [[Configuration]].
10. Create a service file e.g. `/etc/systemd/system/otterwiki.service`

  ```systemd
  [Unit]
  Description=uWSGI server for An Otter Wiki

  [Service]
  User=www-data
  Group=www-data
  WorkingDirectory=/path/to/an/otterwiki
  ExecStart=/path/to/an/otterwiki/env/bin/uwsgi --http 127.0.0.1:8080 -enable-threads --die-on-term -w otterwiki.server:app
  SyslogIdentifier=otterwiki

  [Install]
  WantedBy=multi-user.target
  ```

It's highly recommended to use a webserver as reverse proxy to connect to  uwsgi, see [Reverse Proxy](#reverse-proxy) below.

# Reverse Proxy

A reverse proxy is a server that sits in front of web servers and forwards
client (e.g. web browser) requests to those web servers. They are useful
when hosting multiple services on a host and make it much easier to configure
https. Neither An Otter Wiki itself nor the in the docker image provides https.

Mini how-tos for configuring Apache, NGINX and Caddy are provided below. For complete documention please check the corresponding software documention.

## NGINX

This is a minimal example of a config that configures NGINX as a reverse proxy. The full documentation about NGINX as reverse proxy can be found [here](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/).

It's assumed that An Otter Wiki is running either in a docker container or as a uwsgi process and listening on port 8080.

```nginx
server {
    server_name wiki.domain.tld;
    listen 80;
    location / {
       proxy_set_header Host $http_host;
       proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       proxy_set_header X-Forwarded-Host $http_host;
       proxy_pass http://127.0.0.1:8080;
    }
}
```

### NGINX on Debian, Ubuntu and derivates

- Install nginx via `apt install -y nginx`
- Create the `otterwiki.conf` in `/etc/nginx/sites-enabled/`
- Check the syntax via `nginx -t`
- Restart nginx via `systemctl restart nginx`
- Open <http://wiki.domain.tld> in your browser
- Check `journalctl -xeu nginx` and `/var/log/nginx/error.log` for errors.

### NGINX on RHEL, CentOS, Rocky and derivates

- Install nginx via `dnf install nginx`
- Start NGINX and enable at boot: `sudo systemctl enable --now nginx`
- With SELinux enabled, make sure httpd can connect using `setsebool -P httpd_can_network_connect on`
- Create the `otterwiki.conf` in `/etc/nginx/conf.d/`
- Check the syntax via `nginx -t`
- Restart nginx via `systemctl restart nginx`
- Open <http://wiki.domain.tld> in your browser
- Check `journalctl -xeu nginx` and `/var/log/nginx/error.log` for errors.

See <https://www.redhat.com/sysadmin/setting-reverse-proxies-nginx> for a complete guide.

## Apache

This is a minimal example how to configure Apache as reverse proxy. Please see the [Apache documentation](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html) for complete details.

It's assumed that An Otter Wiki is running either in a docker container or as a uwsgi process and listening on port 8080.

```
<VirtualHost *:*>
  ServerName wiki.domain.tld
  ProxyPreserveHost On
  ProxyPass / http://0.0.0.0:8080/
  ProxyPassReverse / http://0.0.0.0:8080/
</VirtualHost>
```

### Apache on Debian, Ubuntu and derivates

- Install apache2 via `apt install -y apache2`
- Enable proxy modules via `a2enmod proxy proxy_http`
- Create the `otterwiki.conf` config file in `/etc/apache2/site-available/`
- Enable the site via `a2ensite otterwiki`
- Restart apache2 `systemctl restart apache2`
- Open <http://wiki.domain.tld> in your browser
- Check `journalctl -xeu apache2.service` and `/var/log/apache2/error.log` for error messages.

### Apache on RHEL, CentOS, Rocky and derivates

- Install apache2 via `dnf install httpd`
- Start apache2 and enable at boot: `sudo systemctl enable --now httpd`
- Create the `otterwiki.conf` config file in `/etc/httpd/conf.d/`
- With SELinux enabled, make sure httpd can connect using `setsebool -P httpd_can_network_connect on`
- Restart apache2 via `systemctl restart httpd`
- Open <http://wiki.domain.tld> in your browser
- Check `journalctl -xeu httpd.service` and `/var/log/httpd/error_log` for error messages.

## Caddy

[Caddy](https://caddyserver.com/) is an open source webserver with a very light configuration that makes a fine reverse proxy.

After [installing](https://caddyserver.com/docs/install) caddy, configure `/etc/caddy/Caddyfile` with e.g.

```
domain.tld {
    reverse_proxy localhost:8080
}
```

With a server accessible from the internet, `domain.tld` beeing a proper domain name with A/AAAA DNS records pointing to the server, caddy will [automatically](https://caddyserver.com/docs/automatic-https) serve HTTPS.
