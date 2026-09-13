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
name: projectsend
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
      MARIADB_DATABASE: projectsend
      MARIADB_PASSWORD: 98be9a08cd32d820d62d0740b6f294a9WORD
      MARIADB_ROOT_PASSWORD: 6d8db339f5c7ef1366c134aa1383ad3dWORD
      MARIADB_USER: projectsend
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
  projectsend:
    cap_drop:
      - ALL
    container_name: projectsend
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      TZ: Etc/UTC
      UMASK: "002"
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/projectsend:r1945
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
        source: /mnt/tank/apps/projectsend/config
        target: /config
        read_only: false
```
