---
slug: "news/truecharts/common-2026-rollout"
title: "Common-2026 Is Now Rolled Out to All Charts: Migration Required"
authors: [alfi0812]
date: 2026-11-23
tags:
  - "2026"
  - "announcement"
---

## Common-2026 Is Now Live in All Charts

A few days ago we [announced common-2026](/news/truecharts/common-2026/), the biggest rework of our common library so far.
As of today, **all stable and incubator charts have been moved to common-2026**.
Every chart received a **major version increase** for this update.

:::danger[A migration is required for all dependencies]
Every chart that uses a dependency (PostgreSQL/CNPG, Valkey, MariaDB, MongoDB, ClickHouse or Memcached) needs a migration.
**Do not update blindly.** Read this article and the [common-2026 announcement](/news/truecharts/common-2026/) first,
make backups, and make sure you can restore them.
:::

## Valkey, MariaDB and MongoDB: Manual Backup Required

Valkey, MariaDB and MongoDB now run on our own images, use a new volume and another data layout.
Their data is **not migrated** automatically.

:::danger[Your data will be gone after the update]
After the update, these dependencies start with an **empty database**.
You must make a **manual backup before updating**, and restore it after the update.
Without a backup, your data is lost.
:::

1. Make a backup (dump) of your Valkey, MariaDB or MongoDB data **before** updating.
2. Verify that the backup is complete and can be restored.
3. Update the chart.
4. Restore your backup into the new database.

The same applies to ClickHouse.

## PostgreSQL (CNPG): Pin to Version 16 First

CNPG has moved under `dependencies`, and PostgreSQL 18 is now the default.
Clusters can't upgrade directly from the old images to PostgreSQL 18 during this update,
so if you currently use a chart with CNPG, **pin it to PostgreSQL 16 for the update**.

Set this in your values **before** updating the chart:

```yaml
dependencies:
  postgresql:
    operator:
      pgVersion: 16
```

1. Add the `pgVersion: 16` pin to your values.
2. Update the chart.
3. **Wait** until the cluster has migrated and is healthy again.
4. Remove the `pgVersion` pin from your values, and update again.
   The cluster then automatically upgrades to **PostgreSQL 18**.

:::tip
Take a full backup before each step. Older backups can't be used for point-in-time recovery across major PostgreSQL versions,
so take a new backup after the upgrade to 18 as well.
:::

### Backups Now Require the Barman Cloud Plugin

CNPG backups and object store recovery now require the
[CNPG Barman Cloud plugin](https://cloudnative-pg.io/plugin-barman-cloud/docs/installation/),
installed in the namespace of the CloudNativePG operator.
It needs CloudNativePG 1.26 or newer and cert-manager.
Rendering fails when backups or recovery are enabled and the plugin isn't installed.

## Gateway API Support with Envoy and Traefik

All charts now come with complete **Gateway API** support.
Routes are at feature parity with ingress, including gateway selection, per-route TLS with cert-manager,
and integrations for **Envoy Gateway** and **Traefik**.

## Everything Else

This article only covers what you need for the rollout.
For all other changes, including the closed values schema, removed keys, moved documentation and the full changelog,
see the [common-2026 announcement](/news/truecharts/common-2026/).

## Thank You

Thank you to everyone who helped test common-2026, and to all users for their patience during this migration.
If you run into problems, reach out to us through our support channels at [https://trueforge.org](https://trueforge.org).
