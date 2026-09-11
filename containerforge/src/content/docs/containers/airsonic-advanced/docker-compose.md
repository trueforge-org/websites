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
name: airsonic-advanced
services:
  airsonic-advanced:
    cap_drop:
      - ALL
    container_name: airsonic-advanced
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      SPRING_DATASOURCE_DRIVER_CLASS_NAME: org.postgresql.Driver
      SPRING_DATASOURCE_PASSWORD: a62ab2a67ae5418da4ca50ef52ec3c41WORD
      SPRING_DATASOURCE_URL: jdbc:postgresql://postgresql:5432/airsonic-advanced
      SPRING_DATASOURCE_USERNAME: airsonic-advanced
      TZ: Etc/UTC
      UMASK: "002"
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/airsonic-advanced:11.1.4
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 4040
        published: "4040"
        protocol: tcp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 4041
        published: "4041"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/airsonic-advanced/config
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
#       POSTGRES_DB: airsonic-advanced
#       POSTGRES_PASSWORD: a62ab2a67ae5418da4ca50ef52ec3c41WORD
#       POSTGRES_USER: airsonic-advanced
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
