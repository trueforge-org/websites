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
name: readarr
services:
  readarr:
    cap_drop:
      - ALL
    container_name: readarr
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      DB_CACHEDB: readarr-cache
      DB_DATABASE: readarr
      DB_HOST: postgresql
      DB_LOGSDB: readarr-log
      DB_PASSWORD: 74fe3e88b7b049862707849fa72517d0WORD
      DB_PORT: "5432"
      DB_TYPE: sqlite
      DB_USER: readarr
      TZ: Etc/UTC
      UMASK: "002"
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/readarr:0.4.18.2805
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8787
        published: "8787"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/readarr/config
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
#       POSTGRES_DB: readarr
#       POSTGRES_PASSWORD: 74fe3e88b7b049862707849fa72517d0WORD
#       POSTGRES_USER: readarr
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
