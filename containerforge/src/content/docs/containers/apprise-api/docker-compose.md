---
title: Docker-Compose
---

:::warning

These settings are best-effort and will likely require additional work to implement

:::

Every docker-container we build, can be easily loaded using a docker-compose file.

Please note that any dependencies need to be manually connected (primarily their database names, usernames and passwords.
Any optional dependancies or env-vars are commented out.

Please do check the application source for installation instructions and any env-vars and ports that are not managed/created by us.

Source: [{{ SOURCE }}]({{ SOURCE }})

## docker-compose.yaml

```yaml
name: apprise-api
services:
  apprise-api:
    cap_drop:
      - ALL
    container_name: apprise-api
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      APPRISE_ATTACH_SIZE: "0"
      APPRISE_DEFAULT_THEME: default
      APPRISE_STATEFUL_MODE: simple
      LOG_LEVEL: INFO
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/apprise-api:2.0.1
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8000
        published: "8000"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/apprise-api/config
        target: /config
        read_only: false
```
