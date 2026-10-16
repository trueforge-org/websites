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
      DB_PASSWORD: df44b52b670098cf8d32673f7e3b71b6WORD
      DB_PORT: "5432"
      DB_USER: netbox
      REDIS_DB_CACHE: "1"
      REDIS_DB_TASK: "0"
      REDIS_HOST: valkey
      REDIS_PASSWORD: ef1ef90cc2fd7153bcef1a1d62d458edWORD
      REDIS_PORT: "6379"
      SECRET_KEY: ""
      SUPERUSER_EMAIL: ""
      SUPERUSER_NAME: admin
      SUPERUSER_PASSWORD: 4d9f79c23a26634e567d42d79fa4ad90WORD
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/netbox:4.7.3
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
      POSTGRES_PASSWORD: df44b52b670098cf8d32673f7e3b71b6WORD
      POSTGRES_USER: netbox
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
      VALKEY_PASSWORD: ef1ef90cc2fd7153bcef1a1d62d458edWORD
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
```
