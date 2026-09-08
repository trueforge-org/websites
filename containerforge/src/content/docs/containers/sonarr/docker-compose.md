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
name: sonarr
services:
  sonarr:
    cap_drop:
      - ALL
    container_name: sonarr
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      DB_DATABASE: sonarr
      DB_HOST: postgresql
      DB_LOGSDB: sonarr-log
      DB_PASSWORD: 9e4079f3a4afedaca604564fffdb01ffWORD
      DB_PORT: "5432"
      DB_TYPE: sqlite
      DB_USER: sonarr
      TZ: Etc/UTC
      UMASK: "002"
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/sonarr:4.0.19.3011
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8989
        published: "8989"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/sonarr/config
        target: /config
        read_only: false
#   postgresql:
#     cap_drop:
#       - ALL
#     container_name: postgresql
#     deploy:
#       resources:
#         limits:
#           cpus: 4
#           memory: "4294967296"
#     environment:
#       POSTGRES_DB: sonarr
#       POSTGRES_PASSWORD: 9e4079f3a4afedaca604564fffdb01ffWORD
#       POSTGRES_USER: sonarr
#       TZ: Etc/UTC
#     group_add:
#       - "568"
#     image: ghcr.io/trueforge-org/postgresql:18.3
#     ports:
#       - mode: ingress
#         host_ip: 127.0.0.1
#         target: 5432
#         published: "5432"
#         protocol: tcp
#     restart: unless-stopped
#     shm_size: "268435456"
#     volumes:
#       - type: bind
#         source: /mnt/tank/apps/postgresql/config
#         target: /config
```
