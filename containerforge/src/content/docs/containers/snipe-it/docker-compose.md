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
name: snipe-it
services:
  mariadb:
    cap_drop:
      - ALL
    container_name: mariadb
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      MARIADB_DATABASE: snipe-it
      MARIADB_PASSWORD: 4719c637c5d1a602641f02d9c33f1b36WORD
      MARIADB_ROOT_PASSWORD: f36c2fccc1dd2bf0484362bb315bfc28WORD
      MARIADB_USER: snipe-it
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/mariadb:11.4.8-r0
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 3306
        published: "3306"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/mariadb/config
        target: /config
        read_only: false
  snipe-it:
    cap_drop:
      - ALL
    container_name: snipe-it
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      APP_KEY: ""
      APP_LOCALE: en
      APP_TIMEZONE: UTC
      APP_URL: ""
      DB_CONNECTION: mysql
      DB_DATABASE: snipe-it
      DB_HOST: mariadb
      DB_PASSWORD: 4719c637c5d1a602641f02d9c33f1b36WORD
      DB_PORT: "3306"
      DB_USERNAME: snipe-it
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/snipe-it:8.8.3
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 80
        published: "80"
        protocol: tcp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 443
        published: "443"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/snipe-it/config
        target: /config
        read_only: false
```
