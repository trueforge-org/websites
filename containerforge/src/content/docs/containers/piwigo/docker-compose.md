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
name: piwigo
services:
  piwigo:
    cap_drop:
      - ALL
    container_name: piwigo
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
    image: ghcr.io/trueforge-org/piwigo:16.4.0
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
        source: /mnt/tank/apps/piwigo/config
        target: /config
        read_only: false
      - type: bind
        source: /mnt/tank/apps/piwigo/gallery
        target: /gallery
        read_only: false
#   mariadb:
#     cap_drop:
#       - ALL
#     container_name: mariadb
#     deploy:
#       resources:
#         limits:
#           cpus: 4
#           memory: "4294967296"
#     environment:
#       MARIADB_DATABASE: piwigo
#       MARIADB_PASSWORD: b6a18ed1a66237aa92fab3d864be7f2fWORD
#       MARIADB_ROOT_PASSWORD: fcfdcb5cc82bc6f3154f2d9fd2d5920dWORD
#       MARIADB_USER: piwigo
#       TZ: Etc/UTC
#     group_add:
#       - "568"
#     image: ghcr.io/trueforge-org/mariadb:11.4.8-r0
#     ports:
#       - mode: ingress
#         host_ip: 127.0.0.1
#         target: 3306
#         published: "3306"
#         protocol: tcp
#     restart: unless-stopped
#     shm_size: "268435456"
#     volumes:
#       - type: bind
#         source: /mnt/tank/apps/mariadb/config
#         target: /config
```
