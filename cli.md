# Command Line Interface

An Otter Wiki ships a Flask command line interface for user management (from
version **2.18.0**). It is the recovery path when nobody can log in any more or
the mail server is broken, so it works without a browser and without a working
login. Every command carries its own `--help`, for example `flask user --help`
or `flask user create --help`.

## Running the CLI

The environment variable `FLASK_APP=otterwiki.server` must be set. Both docker
images set it already, so inside a container the commands are just
`flask user ...`.

### In Docker

Run the CLI in the running container with `docker compose exec`. Replace
`otterwiki` below with the name of your service.

The full image (`redimp/otterwiki:2`) runs the wiki as the `www-data` user even
though the container itself starts as root. Run the CLI as `www-data` too, so
that database writes and git commits are not left owned by root in `/app-data`:

```
docker compose exec -u www-data otterwiki flask user list
```

> [!NOTE]
> If you set `PUID`/`PGID` on the container, run the CLI as that user instead of
> `www-data`. The slim image (`redimp/otterwiki:2-slim`) already runs as
> `www-data`, so there `-u` is not needed:
> `docker compose exec otterwiki flask user list`.

### From a source install

Export `OTTERWIKI_SETTINGS` so the app finds its configuration, then use the
`flask` from the virtual environment the app runs in:

```
export OTTERWIKI_SETTINGS=/path/to/settings.cfg
venv/bin/flask user list
```

## User management

Users have three independent attributes the CLI can set:

- **Flags** `email_confirmed` and `approved`.
- **Permissions** `read`, `write`, `upload` and `admin`. Granting `admin`
  implies `approved`, `read`, `write` and `upload`.
- A **password**. A user without a password cannot log in.

`--flags` and `--permissions` take comma-separated lists, for example
`--permissions=read,write`.

### List users

```
flask user list          # human-readable table
flask user list --json   # machine-readable
```

### Create a user

`flask user create EMAIL NAME` creates an account. The new user has **no
password** and cannot log in until one is set with `flask user password`.

```
flask user create user@example.com "Jane Doe" --flags=email_confirmed,approved --permissions=read,write
```

Short options `-f` (flags) and `-p` (permissions) also work.

### Set or reset a password

`flask user password EMAIL` takes exactly one of:

- `-i`, `--interactive` prompt for the new password.
- `-g`, `--generate` generate a 12-character password and print it.
- `-r`, `--send-password-reset` email a reset link (requires a configured mail
  server).
- `-d`, `--delete` remove the password, blocking login until a reset.

### Edit a user

`flask user edit EMAIL` changes an account. Provide at least one of
`--new-email`, `--new-name`, `--flags` or `--permissions`.

> [!WARNING]
> `--flags` and `--permissions` **overwrite** the current values rather than
> adding to them. To make someone an admin without dropping their other
> attributes, pass the full set you want.

```
flask user edit user@example.com --new-name="Jane Roe" --permissions=read,write,upload
```

### Delete a user

`flask user delete EMAIL` asks for confirmation first; `-y` / `--confirm`
skips it.

## Recovering admin access

If you are locked out, create a fresh admin account (or re-grant admin to your
own) and generate a password for it:

```
docker compose exec -u www-data otterwiki flask user create you@example.com "You" -p admin
docker compose exec -u www-data otterwiki flask user password you@example.com --generate
```

The second command prints the new password. Log in with it and change it from
your profile. See the [[FAQ|FAQ#i-locked-myself-out]] for the same recipe from
the other direction.
