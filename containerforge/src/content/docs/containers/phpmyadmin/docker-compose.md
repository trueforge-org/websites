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
name: phpmyadmin
services:
  phpmyadmin:
    cap_drop:
      - ALL
    container_name: phpmyadmin
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      MAX_EXECUTION_TIME: "600"
      MEMORY_LIMIT: 512M
      PMA_CONFIG_BASE64: ""
      PMA_USER_CONFIG_BASE64: ""
      TZ: Etc/UTC
      UPLOAD_LIMIT: 8192K
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/phpmyadmin:5.2.3
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
        source: /mnt/tank/apps/phpmyadmin/config
        target: /config
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
#       MARIADB_DATABASE: phpmyadmin
#       MARIADB_PASSWORD: 559f3453650f750d704bed7cec5ef237WORD
#       MARIADB_ROOT_PASSWORD: c78d3843dd0380edcd428c29a9fa6a21WORD
#       MARIADB_USER: phpmyadmin
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
