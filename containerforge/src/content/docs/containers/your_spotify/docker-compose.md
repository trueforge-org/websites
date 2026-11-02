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
name: your_spotify
services:
  your_spotify:
    cap_drop:
      - ALL
    container_name: your_spotify
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      APP_URL: ""
      SPOTIFY_PUBLIC: ""
      SPOTIFY_SECRET: ""
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/your_spotify:1.20.0
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 80
        published: "80"
        protocol: tcp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 443
        published: "443"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/your_spotify/config
        target: /config
        read_only: false
#   mongodb:
#     cap_drop:
#       - ALL
#     container_name: mongodb
#     deploy:
#       resources:
#         limits:
#           cpus: 4
#           memory: "4294967296"
#     environment:
#       MONGODB_DATABASE: MYMONGODBDB
#       MONGODB_PASSWORD: 2e64a3dc2b5379229056a17d758c8440WORD
#       MONGODB_ROOT_PASSWORD: 0d4158c05986922c3bd8bc6eede8e5c0WORD
#       MONGODB_USERNAME: MYMONGODBUSER
#       TZ: Etc/UTC
#     group_add:
#       - "568"
#     image: ghcr.io/trueforge-org/mongodb:9.0.4
#     ports:
#       - mode: ingress
#         host_ip: 127.0.0.1
#         target: 27017
#         published: "27017"
#         protocol: tcp
#     restart: unless-stopped
#     shm_size: "268435456"
#     volumes:
#       - type: bind
#         source: /mnt/tank/apps/mongodb/config
#         target: /config
```
