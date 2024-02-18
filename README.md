
# Disclaimer

I am NOT a python developer. This is pretty much copy of official github integration. These files can be installed from local folder only as no updated package is available in pip repository. Tested just on sentry with single organization

# How to

## Main problem

Provide gitlab SSO for sentry 24 and up

## Register plugin

Create **plugins** folder next to `sentry/enhance-image.example.sh`.

Download or clone this repository and put it inside `plugins` folder (I am using `plugins/sentry-auth-gitlab-0.1.0`).

Copy `enance-image.example.sh` to `enhance-image.sh` end append `pip` line to it. It will look like this:

```bash
#!/bin/bash

# Enhance the base $SENTRY_IMAGE with additional dependencies, plugins - see https://github.com/getsentry/self-hosted#enhance-sentry-image
# For example:
# apt-get update
# apt-get install -y gcc libsasl2-dev python-dev libldap2-dev libssl-dev
# pip install python-ldap
pip install /usr/src/sentry/plugins/sentry-auth-gitlab-0.1.0
```

## Setup

Create a new application under your GitLab.
Enter the **Callback URL** as the prefix to your Sentry installation:

```
http(s?)://sentry.example.com/auth/sso/
```

Once done, grab your API keys and drop them in your ``sentry.conf.py:

```python
GITLAB_APP_ID = "APP-ID"
GITLAB_APP_SECRET = "APP-SECRET"
GITLAB_BASE_DOMAIN = "git.example.com"
```

## Finally
Run `./install.sh` script from root of sentry and reconfigure Sentry. `enhance-image.sh` will be appllied during this process.

## Note

```
  # You can specify scope to "api" in Gitlab's OAuth Application page
  # If you failed to do that, set GITLAB_AUTH_SCOPE = "read_user"
  GITLAB_AUTH_SCOPE = "api"
```
