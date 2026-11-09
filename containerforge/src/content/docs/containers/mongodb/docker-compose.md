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
name: mongodb
services:
  mongodb:
    cap_drop:
      - ALL
    container_name: mongodb
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      MONGODB_DATABASE: MYMONGODBDB
      MONGODB_PASSWORD: b9d31a868b1f10b632251711eb26b85fWORD
      MONGODB_ROOT_PASSWORD: 70a29df894fd5c4491f667329b1cff35WORD
      MONGODB_USERNAME: MYMONGODBUSER
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/mongodb:9.0.4
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 27017
        published: "27017"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/mongodb/config
        target: /config
        read_only: false
```
