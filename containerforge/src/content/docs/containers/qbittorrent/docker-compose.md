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
name: qbittorrent
services:
  qbittorrent:
    cap_drop:
      - ALL
    container_name: qbittorrent
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      QBT_WEBUI_PORT: "8080"
      TZ: Etc/UTC
      UMASK: "002"
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/qbittorrent:5.2.4
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 6881
        published: "6881"
        protocol: tcp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 6881
        published: "6881"
        protocol: udp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8080
        published: "8080"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/qbittorrent/config
        target: /config
        read_only: false
```
