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
name: kimai
services:
  kimai:
    cap_drop:
      - ALL
    container_name: kimai
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      DATABASE_URL: mysql://kimai:f5b1a2bd6e40031555612c9e2c1edfbaWORD@mariadb:3306/kimai
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/kimai:2.71.0
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
        source: /mnt/tank/apps/kimai/config
        target: /config
        read_only: false
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
      MARIADB_DATABASE: kimai
      MARIADB_PASSWORD: f5b1a2bd6e40031555612c9e2c1edfbaWORD
      MARIADB_ROOT_PASSWORD: d968660eeb9dda00a7075899854661ffWORD
      MARIADB_USER: kimai
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
```
