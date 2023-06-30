# ![logo](./logo_chris.png) khris-helm

[![MIT License](https://img.shields.io/github/license/fnndsc/khris-helm)](https://github.com/FNNDSC/khris-helm/blob/main/LICENSE)
[![ci](https://github.com/FNNDSC/khris-helm/actions/workflows/ci.yml/badge.svg)](https://github.com/FNNDSC/khris-helm/actions/workflows/ci.yml)

Production deployment of [_ChRIS_](https://chrisproject.org/) on [_Kubernetes_](https://kubernetes.io/)
using [Helm](https://helm.sh/).

_ChRIS_ is an open-source platform for medical compute. Learn more at https://chrisproject.org

## What's in the box?

This repository provides `chris-cube`, a chart which deploys the _ChRIS_ backend a.k.a. "CUBE."

### Batteries Not Included

`chris-cube` is one part of a software platform. In descending order of importance, one should also deploy/use:

- [pfcon](https://github.com/FNNDSC/pfcon) and [pman](https://github.com/FNNDSC/pman), which provide compute to _CUBE_ (kustomize and/or Helm repo coming soon!).
- [ChRIS\_ui](https://github.com/FNNDSC/ChRIS_ui), a web-app graphical user interface for _ChRIS_
- [`chrisomatic`](https://github.com/FNNDSC/chrisomatic), a command-line tool for provisioning _CUBE_

### Limitations

Currently, `chris-cube` only supports single-replicas, file storage using a volume instead of object storage,
and deployment of Postgres using [`docker.io/library/postgres`](https://hub.docker.com/_/postgres). `chris-cube`
is not HA nor does it have a back-up solution ootb. See the [Roadmap](#roadmap) section.

## Installation

You'll need a Kubernetes (k8s) cluster and a [storage class](https://kubernetes.io/docs/concepts/storage/storage-classes/).

On the client, install `helm`: https://helm.sh/docs/intro/install/

### TODO TODO TODO add installation instructions

### NFS Server Workarounds

A NFS-based storage class (for instance, using [nfs-subdir-external-provisioner](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner))
could require that all (stateful) containers run as a user with a specific UID. This can be achieved by specifying a value for `securityContext`, e.g.

```yaml
securityContext:
  runAsUser: 123456
  runAsGroup: 789
```

## Development

If you already have Docker installed, the easiest way to obtain k8s is [KinD](https://kind.sigs.k8s.io/).
[Install `kind`](https://kind.sigs.k8s.io/docs/user/quick-start/) then run

```shell
kind create cluster --config=testing/kind-with-nodeport.yml
```

### Initial Secret Generation

See https://github.com/helm/helm-www/issues/1259#issuecomment-641558251

### Roadmap

- [ ] CUBE image should support arbitrary UIDs: https://github.com/FNNDSC/ChRIS_ultron_backEnd/issues/514
- [ ] Support [Crunchy Postgres Operator (PGO)](https://github.com/CrunchyData/postgres-operator/)
- [ ] Support object storage for _ChRIS_ files storage, e.g. [OpenStack Swift](https://wiki.openstack.org/wiki/Swift), [NooBaa](https://www.noobaa.io/)

With PGO and object storage support, we won't need to request PVCs directly.
