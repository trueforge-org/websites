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
name: redis-exporter
services:
  redis-exporter:
    cap_drop:
      - ALL
    container_name: redis-exporter
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      REDIS_ADDR: redis://localhost:6379
      REDIS_PASSWORD: ""
      REDIS_USER: ""
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/redis-exporter:1.94.0
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 9121
        published: "9121"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
```
