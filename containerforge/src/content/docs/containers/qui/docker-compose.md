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
name: qui
services:
  qui:
    cap_drop:
      - ALL
    container_name: qui
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/qui:1.29.0
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 7476
        published: "7476"
        protocol: tcp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 7476
        published: "7476"
        protocol: udp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/qui/config
        target: /config
        read_only: false
```
