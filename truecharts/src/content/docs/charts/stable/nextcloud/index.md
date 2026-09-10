---
title: nextcloud
---

![Version: 46.10.0](https://img.shields.io/badge/Version-46.10.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 34.0.3](https://img.shields.io/badge/AppVersion-34.0.3-informational?style=flat-square)

A private cloud server that puts the control and security of your own data back into your hands.

## Chart Sources

- https://github.com/nextcloud/docker
- https://github.com/nextcloud/helm
- https://github.com/trueforge-org/containers/tree/main/apps/nextcloud-fpm
- https://github.com/trueforge-org/containers/tree/main/apps/nextcloud-imaginary
- https://github.com/trueforge-org/containers/tree/main/apps/nextcloud-notify-push
- https://github.com/trueforge-org/containers/tree/main/apps/nginx
- https://github.com/trueforge-org/truecharts/tree/master/charts/stable/nextcloud
- https://github.com/trueforge/truecharts/tree/master/charts/stable/nextcloud
- https://hub.docker.com/r/clamav/clamav
- https://hub.docker.com/r/collabora/code

## Available Documentation

- [**Changing Database Password**](./changingpassword)
- [**Installation Notes**](./installation-notes)
- [**NextCloud Support Policy**](./support)


---

## Readme


### General Info

For more information about this Chart, please check the docs on the TrueCharts [website](https://trueforge.org/truecharts/charts/stable/nextcloud)

**This chart is not maintained by the upstream project and any issues with the chart should be raised [here](https://github.com/trueforge-org/truecharts/issues/new/choose)**

### Installation

#### Helm-Chart installation

To install TrueCharts Helm charts using Helm, you can use our OCI Repository.

`helm install mychart oci://oci.trueforge.org/truecharts/nextcloud`

For more information on how to install TrueCharts Helm charts, checkout the [instructions on the website](https://trueforge.org/truecharts/guides/)

### Chart Specific Guides and information

All our charts have dedicated documentation pages.
The documentation for this chart can be found here:
https://truecharts.org/charts/stable/nextcloud

### Configuration Options

To view the chart specific options, please view Values.yaml included in the chart.
The most recent version of which, is available here: https://github.com/trueforge-org/truecharts/blob/master/charts/stable/nextcloud/values.yaml

All our Charts use a shared "common" library chart that contains most of the templating and options.
For the complete overview of all available options, please checkout the documentation for them on the [common docs on our website](https://trueforge.org/truecharts-common/)

For information about the common chart and all defaults included with it, please review its values.yaml file available here: https://github.com/trueforge-org/truecharts/blob/master/charts/library/common/values.yaml

### Support

- See the [Website](https://truecharts.org)
- Check our [Discord](https://discord.gg/tVsPTHWTtr)
- Open an [issue](https://github.com/trueforge-org/truecharts/issues/new/choose)

---

### Sponsor TrueCharts

TrueCharts can only exist due to the incredible effort of our staff.
Please consider making a [donation](https://trueforge.org/general/sponsor/) or contributing back to the project any way you can!

_All Rights Reserved - The TrueCharts Project_
