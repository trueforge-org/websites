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
name: hedgedoc
services:
  hedgedoc:
    cap_drop:
      - ALL
    container_name: hedgedoc
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      CMD_ALLOW_ANONYMOUS: "true"
      CMD_ALLOW_FREEURL: "false"
      CMD_DB_DIALECT: postgres
      CMD_DOMAIN: ""
      CMD_PROTOCOL_USESSL: "false"
      CMD_SESSION_SECRET: ""
      CMD_URL_ADDPORT: "false"
      DB_HOST: postgresql
      DB_NAME: hedgedoc
      DB_PASS: cf3b8b813f09b682818cd5542b7e2b2aWORD
      DB_PORT: "5432"
      DB_USER: hedgedoc
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/hedgedoc:1.12.0
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
        source: /mnt/tank/apps/hedgedoc/config
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
      POSTGRES_DB: hedgedoc
      POSTGRES_PASSWORD: cf3b8b813f09b682818cd5542b7e2b2aWORD
      POSTGRES_USER: hedgedoc
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
```
