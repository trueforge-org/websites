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
name: sealskin
services:
  sealskin:
    cap_drop:
      - ALL
    container_name: sealskin
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
    image: ghcr.io/trueforge-org/sealskin:0.3.2
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8000
        published: "8000"
        protocol: tcp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8443
        published: "8443"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/sealskin/config
        target: /config
        read_only: false
      - type: bind
        source: /mnt/tank/apps/sealskin/storage
        target: /storage
        read_only: false
```
