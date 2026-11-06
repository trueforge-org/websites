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
name: clickhouse
services:
  clickhouse:
    cap_drop:
      - ALL
    container_name: clickhouse
    privileged: false
    deploy:
      resources:
        limits:
          cpus: 4
          memory: 4G
    environment:
      CLICKHOUSE_DB: MYCLICKHOUSEDB
      CLICKHOUSE_DEFAULT_ACCESS_MANAGEMENT: "0"
      CLICKHOUSE_PASSWORD: d850ce2008539c4783df1a4dc07c22adWORD
      CLICKHOUSE_USER: MYCLICKHOUSEUSER
      TZ: Etc/UTC
    group_add:
      - "568"
    image: ghcr.io/trueforge-org/clickhouse:26.10.2.103
    ports:
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 8123
        published: "8123"
        protocol: http
      - mode: ingress
        # host_ip: 127.0.0.1
        target: 9000
        published: "9000"
        protocol: tcp
    restart: unless-stopped
    shm_size: 256M
    volumes:
      - type: bind
        source: /mnt/tank/apps/clickhouse/config
        target: /config
        read_only: false
```
