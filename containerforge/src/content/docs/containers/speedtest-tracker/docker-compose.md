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
name: speedtest-tracker
services:
  speedtest-tracker:
    cap_drop:
      - ALL
    container_name: speedtest-tracker
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      APP_KEY: ""
      APP_URL: ""
      DB_CONNECTION: sqlite
      DB_DATABASE: ""
      DB_HOST: ""
      DB_PASSWORD: 6aca88d012a46ac9be07550462e40e8cWORD
      DB_PORT: ""
      DB_USERNAME: ""
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/speedtest-tracker:1.15.0
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/speedtest-tracker/config
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
#       POSTGRES_DB: speedtest-tracker
#       POSTGRES_PASSWORD: 6aca88d012a46ac9be07550462e40e8cWORD
#       POSTGRES_USER: speedtest-tracker
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
