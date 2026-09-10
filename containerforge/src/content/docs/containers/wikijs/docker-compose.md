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
name: wikijs
services:
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
      POSTGRES_DB: wikijs
      POSTGRES_PASSWORD: f77d3cf3d6d1fd6d3887bfdae2fe9b1aWORD
      POSTGRES_USER: wikijs
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
  wikijs:
    cap_drop:
      - ALL
    container_name: wikijs
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      DB_HOST: postgresql
      DB_NAME: wikijs
      DB_PASS: f77d3cf3d6d1fd6d3887bfdae2fe9b1aWORD
      DB_PORT: "5432"
      DB_TYPE: postgres
      DB_USER: wikijs
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/wikijs:2.5.314
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 3000
        published: "3000"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/wikijs/config
        target: /config
        read_only: false
```
