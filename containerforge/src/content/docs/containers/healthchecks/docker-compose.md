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
name: healthchecks
services:
  healthchecks:
    cap_drop:
      - ALL
    container_name: healthchecks
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      ALLOWED_HOSTS: '*'
      DEBUG: "False"
      DEFAULT_FROM_EMAIL: ""
      EMAIL_HOST: ""
      EMAIL_HOST_PASSWORD: ""
      EMAIL_HOST_USER: ""
      EMAIL_PORT: "587"
      EMAIL_USE_TLS: "true"
      SECRET_KEY: ""
      SITE_NAME: Mychecks
      SITE_ROOT: ""
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/healthchecks:4.4
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
        source: /mnt/tank/apps/healthchecks/config
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
      POSTGRES_DB: healthchecks
      POSTGRES_PASSWORD: 411eb6c61942f6fa0bf5f35fa5257233WORD
      POSTGRES_USER: healthchecks
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
