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
name: dexafix
services:
  dexafix:
    cap_drop:
      - ALL
    container_name: dexafix
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      DEXAFIX_DSN: file:/config/dexafix.db
      DEXAFIX_ENCRYPTION_KEY_FILE: /config/dexafix.key
      DEXAFIX_INTERNAL_LISTEN: :8081
      DEXAFIX_LISTEN: :8080
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/dexafix:0.0.1
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
        source: /mnt/tank/apps/dexafix/config
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
#       POSTGRES_DB: dexafix
#       POSTGRES_PASSWORD: c397c26e732f31a4080aa3ac23481564WORD
#       POSTGRES_USER: dexafix
#       TZ: Etc/UTC
#     group_add:
#       - "568"
#     image: ghcr.io/trueforge-org/postgresql:18.6
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
#   valkey:
#     cap_drop:
#       - ALL
#     container_name: valkey
#     deploy:
#       resources:
#         limits:
#           cpus: 4
#           memory: "4294967296"
#     environment:
#       TZ: Etc/UTC
#       VALKEY_APPENDONLY: "yes"
#       VALKEY_DATA_DIR: /config
#       VALKEY_PASSWORD: 2aa08a5cad844ecc5ee55d2b2fc40069WORD
#     group_add:
#       - "568"
#     image: ghcr.io/trueforge-org/valkey:9.0.4
#     ports:
#       - mode: ingress
#         host_ip: 127.0.0.1
#         target: 6379
#         published: "6379"
#         protocol: tcp
#     restart: unless-stopped
#     shm_size: "268435456"
#     volumes:
#       - type: bind
#         source: /mnt/tank/apps/valkey/config
#         target: /config
```
