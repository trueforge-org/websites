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
name: changedetection
services:
  changedetection:
    cap_drop:
      - ALL
    container_name: changedetection
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      BASE_URL: ""
      PLAYWRIGHT_DRIVER_URL: ""
      TZ: Etc/UTC
      USE_X_SETTINGS_FILE: ""
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/changedetection:0.60.4
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 5000
        published: "5000"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/changedetection/config
        target: /config
        read_only: false
```
