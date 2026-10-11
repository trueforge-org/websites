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
name: syncthing
services:
  syncthing:
    cap_drop:
      - ALL
    container_name: syncthing
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      STGUIADDRESS: 0.0.0.0:8384
      TZ: Etc/UTC
      UMASK: "002"
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/syncthing:2.1.5
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8384
        published: "8384"
        protocol: tcp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 21027
        published: "21027"
        protocol: udp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 22000
        published: "22000"
        protocol: tcp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 22000
        published: "22000"
        protocol: udp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/syncthing/config
        target: /config
        read_only: false
```
