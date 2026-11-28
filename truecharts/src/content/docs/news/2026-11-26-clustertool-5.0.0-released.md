---
slug: "news/truecharts/clustertool-5.0.0-rollout"
title: "ClusterTool 5.0.0 is out!"
authors: [boemeltrein]
date: 2026-11-26
tags:
  - "2026"
  - "announcement"
---

# 🚀 ClusterTool 5.0.0 is out!

This is the biggest ClusterTool release yet. We replaced Talhelper with native Talos configuration and redesigned how clusters are generated, bootstrapped and managed. The release also introduces a single-cluster repository layout, configurable bootstrap and Flux Operator. ClusterTool now supports GitLab and self-hosted Git, and replaces ingress-nginx with Envoy Gateway.

> ⚠️ **This is a breaking release.** There is no automatic migration from 4.x. We recommend bootstrapping a fresh cluster and **backing up all persistent data first**. Please read the breaking changes below before upgrading.

> 📣 **Using TrueCharts Common-2026 with CNPG?** Before removing or restoring CNPG apps, check the [migration notes](https://truecharts.org/news/truecharts/common-2026-rollout) and TrueCharts Discord for the latest backup and restore guidance.

---

## ✨ Highlights

### 🧱 Native Talos configuration – goodbye Talhelper
ClusterTool now generates and validates Talos configs itself from `talos/clustertool.yaml`.

- Patches are organized as **shared**, **role-specific** and **node-specific**
- The bootstrap node is picked automatically (first control-plane node, alphabetically)

### 📦 Flexible application bootstrap
You now decide exactly what gets installed during bootstrap, and in which order:

- `bootstrap.crd` for CRDs that have to be applied early
- `bootstrap.steps` for ordered chart and manifest steps

### 🗂️ One cluster per repository
A cleaner layout: `talos/` for Talos and `kubernetes/` for everything else.

Set `clusterName` in `talos/clustertool.yaml`, independently of repository paths.

### 🔄 Flux Operator
Bootstrap now uses **Flux Operator + FluxInstance** instead of raw Flux manifests.

### 🌐 GitLab & self-hosted Git
GitHub, GitLab and self-hosted Git all work, including custom SSH hosts, ports and paths.

### 🚪 Envoy Gateway replaces ingress-nginx
Gateway API is now the default: internal and external Gateways, wildcard TLS, HTTPS redirects and cert-manager integration out of the box.

---

## 🛠️ More improvements

- **New templates:** CloudNativePG (with the Barman Cloud plugin), Valkey Operator, plus Envoy Gateway / Gateway API resources. Bundled chart versions are updated.
- **New commands:** `genks` (alias `kustomizations`) generates missing Kustomization files. `githook` reinstalls the pre-commit hook.
- **SOPS:** standardized `deploy-key.sops.yaml` and `sops-age.sops.yaml`, support for binary files, and the `stores` / `mac_only_encrypted` settings are now respected
- **Git hooks:** an optional `.git/hooks/pre-commit.local` runs after the encryption checks, plus several hook and staging fixes
- **Reliability:** init asks before running again and keeps its retry state on failure, and `KUBECONFIG` is consistently respected by ClusterTool’s Kubernetes clients
- **Drift detection:** HelmRelease drift detection is optional (`warn` or `enabled`)
- **Releases:** build metadata is included in archives, and Windows builds now ship as ZIP

---

## 💥 Breaking changes – read before upgrading

- ❌ No automatic migration. Fresh bootstrap recommended, **back up your data first**
- Talhelper configs no longer work as-is; use `talos/clustertool.yaml`
- One cluster per repo: `--cluster` and `clusters/<cluster>/` are gone
- `bootstrap.crd` and `bootstrap.steps` are **required** in the config
- Longhorn and OpenEBS storage paths changed. **Existing data is not moved**
- `GITHUB_REPOSITORY` → `GIT_REPOSITORY`
- ingress-nginx → Envoy Gateway (`NGINX_*` → `ENVOY_*`)
- `cluster-settings.sops.yaml` → `kubernetes/config/cluster-secrets.sops.yaml`
- Numeric and boolean values in settings **must be quoted**

---

📥 **Download:** https://github.com/trueforge-org/clustertool/releases/tag/v5.0.0

📜 **Full changelog:** https://github.com/trueforge-org/clustertool/compare/v4.0.0...v5.0.0

A huge thank you to everyone who contributed, tested and reported issues! 💙 If you run into problems, open an issue or come talk to us on Discord.
