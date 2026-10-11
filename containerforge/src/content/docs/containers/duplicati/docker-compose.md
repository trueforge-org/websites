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
name: duplicati
services:
  duplicati:
    cap_drop:
      - ALL
    container_name: duplicati
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      DUPLICATI__REQUIRE_DB_ENCRYPTION_KEY: "true"
      DUPLICATI__WEBSERVICE_ALLOWED_HOSTNAMES: '*'
      DUPLICATI__WEBSERVICE_INTERFACE: any
      TZ: Etc/UTC
      UMASK: "002"
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/duplicati:2.4.0.1_stable_2026-10-07
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8200
        published: "8200"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/duplicati/backups
        target: /backups
        read_only: false
      - type: bind
        source: /mnt/tank/apps/duplicati/config
        target: /config
        read_only: false
      - type: bind
        source: /mnt/tank/apps/duplicati/source
        target: /source
        read_only: false
```
