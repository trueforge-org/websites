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
name: plex
services:
  plex:
    cap_drop:
      - ALL
    container_name: plex
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      ADVERTISE_IP: ""
      ALLOWED_NETWORKS: ""
      NVIDIA_DRIVER_CAPABILITIES: compute,video,utility
      PLEX_CLAIM: ""
      TZ: Etc/UTC
      UMASK: "002"
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/plex:1.43.4.10903-e5521bd8c
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 1900
        published: "1900"
        protocol: udp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 5353
        published: "5353"
        protocol: udp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8324
        published: "8324"
        protocol: tcp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 32400
        published: "32400"
        protocol: tcp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 32410
        published: "32410"
        protocol: udp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 32412
        published: "32412"
        protocol: udp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 32413
        published: "32413"
        protocol: udp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 32414
        published: "32414"
        protocol: udp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 32469
        published: "32469"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/plex/config
        target: /config
        read_only: false
```
