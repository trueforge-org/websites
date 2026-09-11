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
name: nextcloud-notify-push
services:
  nextcloud-fpm:
    cap_drop:
      - ALL
    container_name: nextcloud-fpm
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      NX_POSTGRES_HOST: postgresql
      NX_POSTGRES_NAME: nextcloud-notify-push
      NX_POSTGRES_PASSWORD: d8a784e2f546ab247e56d034e4bf350bWORD
      NX_POSTGRES_PORT: "5432"
      NX_POSTGRES_USER: nextcloud-notify-push
      NX_REDIS_HOST: valkey
      NX_REDIS_PASS: 77c125a9ca386e9de8fe363b29b241afWORD
      NX_REDIS_PORT: "6379"
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/nextcloud-fpm:34.0.3-fpm
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/nextcloud-fpm/config
        target: /config
        read_only: false
  nextcloud-notify-push:
    cap_drop:
      - ALL
    container_name: nextcloud-notify-push
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/nextcloud-notify-push:1.4.0
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/nextcloud-notify-push/config
        target: /config
        read_only: false
  postgresql:
    cap_drop:
      - ALL
    container_name: postgresql
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      POSTGRES_DB: nextcloud-notify-push
      POSTGRES_PASSWORD: d8a784e2f546ab247e56d034e4bf350bWORD
      POSTGRES_USER: nextcloud-notify-push
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/postgresql:18.3
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
        source: /mnt/tank/apps/postgresql/config
        target: /config
        read_only: false
  valkey:
    cap_drop:
      - ALL
    container_name: valkey
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      TZ: Etc/UTC
      VALKEY_PASSWORD: 77c125a9ca386e9de8fe363b29b241afWORD
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/valkey:9.0.3
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 6379
        published: "6379"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/valkey/config
        target: /config
        read_only: false
```
