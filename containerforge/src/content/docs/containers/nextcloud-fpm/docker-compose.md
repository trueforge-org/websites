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
name: nextcloud-fpm
services:
  nextcloud-fpm:
    cap_drop:
      - ALL
    container_name: nextcloud-fpm
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      NX_POSTGRES_HOST: postgresql
      NX_POSTGRES_NAME: nextcloud-fpm
      NX_POSTGRES_PASSWORD: 038976b4de7c3bc0d48aed6269395821WORD
      NX_POSTGRES_PORT: "5432"
      NX_POSTGRES_USER: nextcloud-fpm
      NX_REDIS_HOST: valkey
      NX_REDIS_PASS: fb49993decfb0be8e13482efa83b0e33WORD
      NX_REDIS_PORT: "6379"
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/nextcloud-fpm:34.0.3-fpm
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/nextcloud-fpm/config
        target: /config
        read_only: false
  postgresql:
    cap_drop:
      - ALL
    container_name: postgresql
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      POSTGRES_DB: nextcloud-fpm
      POSTGRES_PASSWORD: 038976b4de7c3bc0d48aed6269395821WORD
      POSTGRES_USER: nextcloud-fpm
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/postgresql:18.3
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 5432
        published: "5432"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/postgresql/config
        target: /config
        read_only: false
  valkey:
    cap_drop:
      - ALL
    container_name: valkey
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      TZ: Etc/UTC
      VALKEY_PASSWORD: fb49993decfb0be8e13482efa83b0e33WORD
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/valkey:9.0.3
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 6379
        published: "6379"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/valkey/config
        target: /config
        read_only: false
#   memcache:
#     cap_drop:
#       - ALL
#     container_name: memcache
#     deploy:
#       resources:
#         limits:
#           cpus: 4
#           memory: "4294967296"
#     environment:
#       TZ: Etc/UTC
#     group_add:
#       - "568"
#     image: ghcr.io/trueforge-org/memcache:1.6.40-1
#     ports:
#       - mode: ingress
#         host_ip: 127.0.0.1
#         target: 11211
#         published: "11211"
#         protocol: tcp
#     restart: unless-stopped
#     shm_size: "268435456"
#     volumes:
#       - type: bind
#         source: /mnt/tank/apps/memcache/config
#         target: /config
```
