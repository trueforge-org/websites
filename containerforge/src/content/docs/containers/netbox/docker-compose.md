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
name: netbox
services:
  netbox:
    cap_drop:
      - ALL
    container_name: netbox
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      ALLOWED_HOSTS: '*'
      DB_HOST: postgresql
      DB_NAME: netbox
      DB_PASSWORD: e33c9038c578dfa8b21b65c1334bfba5WORD
      DB_PORT: "5432"
      DB_USER: netbox
      REDIS_DB_CACHE: "1"
      REDIS_DB_TASK: "0"
      REDIS_HOST: valkey
      REDIS_PASSWORD: 00ba2d725de2d2efbcf9ad6597008eadWORD
      REDIS_PORT: "6379"
      SECRET_KEY: ""
      SUPERUSER_EMAIL: ""
      SUPERUSER_NAME: admin
      SUPERUSER_PASSWORD: 775093f23df93ad311be4efe661644c8WORD
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/netbox:4.7.5
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8000
        published: "8000"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/netbox/config
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
      POSTGRES_DB: netbox
      POSTGRES_PASSWORD: e33c9038c578dfa8b21b65c1334bfba5WORD
      POSTGRES_USER: netbox
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/postgresql:18.6
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
      VALKEY_APPENDONLY: "yes"
      VALKEY_DATA_DIR: /config
      VALKEY_PASSWORD: 00ba2d725de2d2efbcf9ad6597008eadWORD
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/valkey:9.0.4
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
```
