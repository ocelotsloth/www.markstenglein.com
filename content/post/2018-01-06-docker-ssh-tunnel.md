---
title: Use docker to tunnel insecure api calls over the internet.
author: Mark Stenglein
date: '2018-01-06'
categories:
  - Development
  - Systems Administration
tags:
  - Docker
  - SSH
  - Programming
---

The school I work for just migrated our Powerschool server offsite, introducing an issue for our internal applications which relied on the ODBC API to pull the class schedules for each day. Because the API does not employ encryption, simply opening the port on the remote server was not an option.

SSH provides the ability to tunnel network traffic through an encrypted SSH session. By binding this tunnel to the internal computer’s network interface, any other computer can simply connect to the proxy host. The tunnel presents the remote ports transparently to our application.

For simple experimentation, the following command can be used:

```bash
ssh $REMOTE_URL \
    -l $SSH_USER \
    -L 0.0.0.0:$REMOTE_PROXY_PORT:$REMOTE_HOST:$LOCAL_PROXY_PORT
```

For the remote host and port, you can specify any computer accessable from the remote server you are connecting to over SSH. In the simplest case, this would simply be localhost, but for more complicated situations you can specify other hostnames or ip addresses. Since Powerschool runs on Windows Server, my use case required me to tunnel through a linux host to the Powerschool server.

## Docker Makes Things Better

Manually opening an ssh tunnel is really quite annoying. Wrapping this function into a docker container allows for error recovery as well as simpler configuration. For this reason, I wrote a docker image which accomplishes this task. Find it at https://github.com/ocelotsloth/docker-ssh-tunnel.

The easiest way to use this is by creating a docker-compose file which orchestrates this with your other applications. This is especially useful if your other applications are run through docker, as you can get away with only exposing the tunneled port to the internal docker network.

Here is what that docker-compose file could look like:

```yaml
# docker-compose.yml
version: '2'
services:
  proxy:
    image: 'ocelotsloth/ssh-tunnel:v0.1.0'
    environment:
      - SSH_PASS=${PROXY_SSH_PASS}
      - SSH_URL=${PROXY_SSH_URL}
      - SSH_PORT=${PROXY_SSH_PORT}
      - SSH_USER=${PROXY_SSH_USER}
      - REMOTE_PROXY_PORT=${PROXY_REMOTE_PROXY_PORT}
      - REMOTE_HOST=${PROXY_REMOTE_HOST}
      - LOCAL_PROXY_PORT=${PROXY_LOCAL_PROXY_PORT}
    expose:
      - "${PROXY_LOCAL_PROXY_PORT}:${PROXY_LOCAL_PROXY_PORT}"
    ports:
      - "${PROXY_LOCAL_PROXY_PORT}:${PROXY_LOCAL_PROXY_PORT}"
    restart: always
```

With this file, a simple `docker-compose up -d` would bring up the proxy and configure it to restart (the same way enabling a systemd unit works). You will need the following `.env` file in the same directory as the docker-compose file to pass the configuration values. Alternatively you could just enter them directly.

```bash
PROXY_SSH_PASS=
PROXY_SSH_URL=
PROXY_SSH_PORT=
PROXY_SSH_USER=
PROXY_REMOTE_PROXY_PORT=
PROXY_REMOTE_HOST=
PROXY_LOCAL_PROXY_PORT=
```

Admittedly this has a limited use case, but still useful in just the right circumstances.
