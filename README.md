Nimble streaming server
=======================

This container provides a [nimble](https://es.wmspanel.com/nimble) streaming server listening on ports **8081** (http) and **1935** (RTMP).

Nimble is "freemium" software, it is freely distributed but must be managed through the [WMSPanel Portal](https://wmspanel.com/). You will need to get an account to be able to use this application.

Usage
-----

From Docker registry:

```
docker pull rjrivero/nimble
```

Or build yourself:

```
git clone https://github.com/rjrivero/docker-nimble.git
docker build --rm -t rjrivero/nimble docker-nimble
```

Running the image:

```
docker run --rm -p 8081:8081 -p 1935:1935 \
    -h instance-hostname \
    -e WMSPANEL_USER=your@user.name \
    -e WMSPANEL_PASS=your_password  \
    --name nimble rjrivero/nimble
```

Configuration
-------------

All configuration is managed through the WMSPanel portal. The first time you run the container, you must provide your portal credentials to have the server register itself. You provide the credentials through the following environment variables:

  - **WMSPANEL_USER**: WMSPanel portal username, e.g. your@email.com
  - **WMSPANEL_PASS**: WMSPanel password.
  - **WMSPANEL_SLICES**: Optional, list of slices to register this server to.

The container registers the server and stores all configuration settings under */etc/nimble/*. You may want to have that path mounted from an external volume.

Once */etc/nimble/nimble.conf* is populated with the right account and key data, you no longer need to provide the username and password as environment variables to the container.

If you change your username and/or password, and need to register the server again, you should remove the */etc/nimble/nimble.conf* file and restart the container, providing the new user and pass.

Volumes
-------

You may want to mount to external volumes to the following paths:

  - */etc/nimble*: This is the configuration folder. It is bootstrapped the first time you run the container with a WMSPanel username and password.
  - */var/cache/nimble*: This is used to save temporary files and stats.

The ownership of both paths is changed to the user **nimble** before starting the service.

Ports
-----

The container exposes ports **8081** (HTTP) and **1935** (RTMP)
