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
name: timescaledb15
services:
  timescaledb15:
    cap_drop:
      - ALL
    container_name: timescaledb15
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      PGDATA: /config/data
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/timescaledb15:15.19
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 5432
        published: "5432"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/timescaledb15/config
        target: /config
        read_only: false
```
