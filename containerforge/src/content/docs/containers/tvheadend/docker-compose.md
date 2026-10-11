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
name: tvheadend
services:
  tvheadend:
    cap_drop:
      - ALL
    container_name: tvheadend
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      RUN_OPTS: ""
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/tvheadend:a87be591fa5f1b0ec9223abe2c52269bcbf31156
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 9981
        published: "9981"
        protocol: tcp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 9982
        published: "9982"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/tvheadend/config
        target: /config
        read_only: false
```
