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
name: xbackbone
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
      MARIADB_DATABASE: xbackbone
      MARIADB_PASSWORD: e0a89706586221b866518d60fd4e6797WORD
      MARIADB_ROOT_PASSWORD: 2909e9c3d753f913ddb713555863aa66WORD
      MARIADB_USER: xbackbone
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
  xbackbone:
    cap_drop:
      - ALL
    container_name: xbackbone
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      DB_CONNECTION: mysql
      DB_DATABASE: xbackbone
      DB_HOST: mariadb
      DB_PASSWORD: e0a89706586221b866518d60fd4e6797WORD
      DB_PORT: "3306"
      DB_USERNAME: xbackbone
      TZ: Etc/UTC
      UMASK: "002"
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/xbackbone:3.8.2
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
        source: /mnt/tank/apps/xbackbone/config
        target: /config
        read_only: false
```
