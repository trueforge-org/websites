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
name: mastodon
services:
  mastodon:
    cap_drop:
      - ALL
    container_name: mastodon
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      ACTIVE_RECORD_ENCRYPTION_DETERMINISTIC_KEY: ""
      ACTIVE_RECORD_ENCRYPTION_KEY_DERIVATION_SALT: ""
      ACTIVE_RECORD_ENCRYPTION_PRIMARY_KEY: ""
      DATABASE_URL: postgresql://mastodon:92107318d532637f68b36982c4bb0796WORD@postgresql:5432/mastodon
      OTP_SECRET: ""
      REDIS_URL: redis://:ba119794c4e2ddb1e7f17b2801eb6feeWORD@valkey:6379
      RUN_DB_PREPARE: "false"
      SECRET_KEY_BASE: ""
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/mastodon:4.7.2
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
        source: /mnt/tank/apps/mastodon/config
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
#       POSTGRES_DB: mastodon
#       POSTGRES_PASSWORD: 92107318d532637f68b36982c4bb0796WORD
#       POSTGRES_USER: mastodon
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
