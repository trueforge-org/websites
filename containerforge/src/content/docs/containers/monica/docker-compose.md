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
name: monica
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
      MARIADB_DATABASE: monica
      MARIADB_PASSWORD: d410b0bfc4f8dfd4bf4ac82e1b8eb51fWORD
      MARIADB_ROOT_PASSWORD: d3bcd4c8bdb572c1cf53c0f85b8e78c1WORD
      MARIADB_USER: monica
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/mariadb:11.8.6
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
  monica:
    cap_drop:
      - ALL
    container_name: monica
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      APP_ENV: production
      APP_KEY: ""
      APP_URL: ""
      DB_CONNECTION: mysql
      DB_DATABASE: monica
      DB_HOST: mariadb
      DB_PASSWORD: d410b0bfc4f8dfd4bf4ac82e1b8eb51fWORD
      DB_PORT: "3306"
      DB_USERNAME: monica
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/monica:4.1.2
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
        source: /mnt/tank/apps/monica/config
        target: /config
        read_only: false
```
