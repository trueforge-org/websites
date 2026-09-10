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
name: hishtory-server
services:
  hishtory-server:
    cap_drop:
      - ALL
    container_name: hishtory-server
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      HISHTORY_POSTGRES_DB: postgresql://hishtory-server:6bc41052f9934e8aaaeebb6bf7c17bd7WORD@postgresql:5432/hishtory-server
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/hishtory-server:0.335
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8080
        published: "8080"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/hishtory-server/config
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
      POSTGRES_DB: hishtory-server
      POSTGRES_PASSWORD: 6bc41052f9934e8aaaeebb6bf7c17bd7WORD
      POSTGRES_USER: hishtory-server
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
