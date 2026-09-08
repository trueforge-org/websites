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
name: emby
services:
  emby:
    cap_drop:
      - ALL
    container_name: emby
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      NVIDIA_DRIVER_CAPABILITIES: compute,video,utility
      PUBLISHED_SERVER_URL: ""
      TZ: Etc/UTC
      UMASK: "002"
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/emby:4.10.0.40
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 1900
        published: "1900"
        protocol: udp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 7359
        published: "7359"
        protocol: udp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8096
        published: "8096"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/emby/config
        target: /config
        read_only: false
```
