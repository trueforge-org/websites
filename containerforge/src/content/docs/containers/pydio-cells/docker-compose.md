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
name: pydio-cells
services:
  pydio-cells:
    cap_drop:
      - ALL
    container_name: pydio-cells
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      EXTERNALURL: https://localhost:8080
      SERVER_IP: 0.0.0.0
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/pydio-cells:5.0.2
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
        source: /mnt/tank/apps/pydio-cells/config
        target: /config
        read_only: false
```
