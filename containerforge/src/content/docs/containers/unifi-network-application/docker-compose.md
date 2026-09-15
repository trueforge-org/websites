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
name: unifi-network-application
services:
  unifi-network-application:
    cap_drop:
      - ALL
    container_name: unifi-network-application
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
    image: ghcr.io/trueforge-org/unifi-network-application:10.6.106
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8080
        published: "8080"
        protocol: tcp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8443
        published: "8443"
        protocol: tcp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8843
        published: "8843"
        protocol: tcp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8880
        published: "8880"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/unifi-network-application/config
        target: /config
        read_only: false
```
