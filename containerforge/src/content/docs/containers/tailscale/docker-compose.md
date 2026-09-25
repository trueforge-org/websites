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
name: tailscale
services:
  tailscale:
    cap_drop:
      - ALL
    container_name: tailscale
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      TS_ACCEPT_DNS: "false"
      TS_AUTHKEY: ""
      TS_EXTRA_ARGS: ""
      TS_HOSTNAME: ""
      TS_ROUTES: ""
      TS_USERSPACE: "true"
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/tailscale:v1.102.5
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/tailscale/config
        target: /config
        read_only: false
      - type: bind
        source: /mnt/tank/apps/tailscale/tailscale
        target: /var/lib/tailscale
        read_only: false
```
