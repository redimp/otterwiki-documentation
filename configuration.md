# Configuration

The [[Installation Guide|Installation]] can be found [[here|Installation]].

An Otter Wiki is configured in the application via the <i class="fas fa-cogs"></i>
 **Settings** interface as admin user. Alternatively you configure the variables via the
`settings.cfg` or via environment variables.

*Please note:* What is set in the config file `settings.cfg` will be overwritten first
by the environment variables if they are set and second by the settings configured
via the settings interface, which are stored in the database. In brief: `Settings Interface > Environment Variables > settings.cfg`.

### Branding

| Variable         |  Example        | Description                                  |
|------------------|-----------------|----------------------------------------------|
| `SITE_NAME`      | `'Otterwiki'`   | The `SITE_NAME` displayed on every page and email |
| `SITE_LOGO`      | `'/Home/a/logo.png'` | Customize navbar logo url (can be a page attachment) |
| `SITE_DESCRIPTION` | `'A minimalistic wiki powered by python, markdown and git.'` | The default description used in `<meta>` tags |
| `SITE_ICON`      | `'/Home/a/favicon-32x32.png'` | Configure via an url to the image that is displayed as favicon (tab icon, URL icon, bookmark icon). This can be an attachment |


### Permission configuration

| Variable         |  Example        | Description                                  |
|------------------|-----------------|----------------------------------------------|
| `READ_ACCESS`    | `'ANONYMOUS'`   | Read access to wiki pages and attachments    |
| `WRITE_ACCESS`   | `'REGISTERED'`  | Write access to wiki pages                   |
| `ATTACHMENT_ACCESS` | `'APPROVED'` | Write acccess to attachments                 |
| `DISABLE_REGISTRATION` | `False` | With `DISABLE_REGISTRATION=True` new users can not sign-up for a new account |
| `AUTO_APPROVAL`  | `False`         | With `AUTO_APPROVAL=True` users are approved on registration |
| `EMAIL_NEEDS_CONFIRMATION`  | `True`         | With `EMAIL_NEEDS_CONFIRMATION=True` users have to confirm their email address |
| `NOTIFY_ADMINS_ON_REGISTER` | `True`  | Notify admins if a new user is registered |

There are four types of users in the Otterwiki: `ANONYMOUS` are non logged in users.
Users that registered via email and are logged in are `REGISTERED`, users approved via
the settings menu by an admin are `APPROVED`. In addition to the `APPROVED` flag the `ADMIN`
flag can be set. Users with the `ADMIN` flag can edit (and approve) other users. The first registered user is flagged as admin.


### Sidebar Preferences

| Variable                | Example    | Description    |
| ----------------------- | ---------- | -------------- |
| `SIDEBAR_MENUTREE_MODE` | `'SORTED'` | Mode of the sidebar, see below. |
| `SIDEBAR_MENUTREE_MAXDEPTH` | `unlimited` | Limit the depth of the pages displayed by any number. |

For `SIDEBAR_MENUTREE_MODE` pick one of

- `NONE` (or empty) no sidebar displayed
- `SORTED` Directories and pages, sorted
- `DIRECTORIES_GROUPED` Directories and pages, with directories grouped first
- `DIRECTORIES_ONLY`List directories only.

### Content and Editing Preferences

| Variable                | Example    | Description    |
| ----------------------- | ---------- | -------------- |
| `COMMIT_MESSAGE` | `'REQUIRED'` | set `COMMIT_MESSAGE='OPTIONAL'` if commit messages are optional |
| `RETAIN_PAGE_NAME_CASE` | `False` | set `RETAIN_PAGE_NAME_CASE=True` to
| `GIT_WEB_SERVER` | `False` | Set to to true to allow cloning the wiki via git+http(s) |

### Mail configuration

An Otter Wiki is using [Flask-Mail](https://pythonhosted.org/Flask-Mail/).

| Variable         |  Example        | Description                                  |
|------------------|-----------------|----------------------------------------------|
| `MAIL_DEFAULT_SENDER` | `'otterwiki@example.com'` | The sender address of all mails |
| `MAIL_SERVER`    | `'smtp.googlemail.com'` | The smtp server address              |
| `MAIL_PORT`      | `465`           | The smtp server port                         |
| `MAIL_USERNAME`  | `'USERNAME'`    | Username for the mail account                |
| `MAIL_PASSWORD`  | `'PASSWORD'`    | Password for the mail account                |
| `MAIL_USE_TLS`   | `False`         | Use TLS encrytion                            |
| `MAIL_USE_SSL`   | `True`          | Use SSL encryption                           |

### Authentication configuration

| Variable         |  Example        | Description                                  |
|------------------|-----------------|----------------------------------------------|
| `AUTH_METHOD` | `'SIMPLE'` | See below. |

Per default an Otter Wiki uses a local database for storing authentication information.

#### Authentication with `PROXY_HEADER`s

With `AUTH_METHOD='PROXY_HEADER'` an Otter Wiki expects the headers

- `x-otterwiki-name`
- `x-otterwiki-email`
- `x-otterwiki-permissions`

to be set by the proxy service using forward authentication.

The headers `x-otterwiki-name`and `x-otterwiki-email` are used for receiving author information and `x-otterwiki-permissions` a comma seperated list of permissions `READ`, `WRITE`, `UPLOAD` and `ADMIN`.

A simplified proof of concept can be found on github: [otterwiki/docs/auth_examples/header-auth](https://github.com/redimp/otterwiki/tree/main/docs/auth_examples/header-auth).

### Advanced configuration

This applies only when you create the `settings.cfg` manually. Create your
`settings.cfg` based upon the `settings.cfg.skeleton` and set the
variables fitting to your environment.

| Variable         |  Example        | Description                                  |
|------------------|-----------------|----------------------------------------------|
| `SECRET_KEY`     | `'CHANGE ME'`   | Choose a random string that is used to encrypt user session data |
| `REPOSITORY`     | `'/path/to/the/repository/root'` | The absolute path to the repository storing the wiki pages |
| `SQLALCHEMY_DATABASE_URI` | `'sqlite:////path/to/the/sqlite/file'` | The absolute path to the database storing the user credentials |

For the `SQLALCHEMY_DATABASE_URI` see <https://flask-sqlalchemy.palletsprojects.com/en/2.x/config/#connection-uri-format>.

### Reverse Proxy and IPs

Running the docker container behind a reverse proxy will show only the IP of the reverse proxy in the log files. With setting `REAL_IP_FROM` to the ip address of the reverse proxy, the IPs of the connection clients will be logged.

| Variable         |  Example         | Description                                  |
|------------------|------------------|----------------------------------------------|
| `REAL_IP_FROM`   | `'10.0.0.0/8'`   | Configure nginx to respect `real_ip_header`, see <http://nginx.org/en/docs/http/ngx_http_realip_module.html> |
