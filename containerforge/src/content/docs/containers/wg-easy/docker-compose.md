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
name: wg-easy
services:
  wg-easy:
    cap_drop:
      - ALL
    container_name: wg-easy
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      DEBUG: Server,WireGuard,Database,CMD,Firewall
      DISABLE_IPV6: "false"
      HOST: 0.0.0.0
      INIT_ENABLED: "false"
      INSECURE: "false"
      PORT: "51821"
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/wg-easy:15.5.0
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 51820
        published: "51820"
        protocol: udp
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 51821
        published: "51821"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/wg-easy/wireguard
        target: /etc/wireguard
        read_only: false
```
