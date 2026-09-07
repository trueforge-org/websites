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
name: openssh-server
services:
  openssh-server:
    cap_drop:
      - ALL
    container_name: openssh-server
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      PASSWORD_ACCESS: "false"
      PUBLIC_KEY: ""
      SUDO_ACCESS: "false"
      TZ: Etc/UTC
      USER_NAME: apps
      USER_PASSWORD: 787fec30a34486e5ca70eb771c3c68aaWORD
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/openssh-server:10.2_p1-r0-ls217
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 2222
        published: "2222"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/openssh-server/config
        target: /config
        read_only: false
```
