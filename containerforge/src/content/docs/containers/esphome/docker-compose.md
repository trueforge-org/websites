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
name: esphome
services:
  esphome:
    cap_drop:
      - ALL
    container_name: esphome
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      ESPHOME_DASHBOARD_RELATIVE_URL: /
      ESPHOME_DASHBOARD_USE_PING: "false"
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/esphome:2026.8.2
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 5353
        published: "5353"
        protocol: udp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 6052
        published: "6052"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/esphome/config
        target: /config
        read_only: false
```
