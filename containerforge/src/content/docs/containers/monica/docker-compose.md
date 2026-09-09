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
      MARIADB_PASSWORD: 15efa6f69a404ebae4ccea75bd5e3de0WORD
      MARIADB_ROOT_PASSWORD: f680ed2ac6577566ad3f4365c93e9afbWORD
      MARIADB_USER: monica
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
      DB_PASSWORD: 15efa6f69a404ebae4ccea75bd5e3de0WORD
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
