# w8s reference architectures

Complete apps built for Kubernetes clusters connected through
[w8s.cloud](https://w8s.cloud), ready to deploy across several of them.

Everything here shows up in the w8s app under **Apps → Reference
architectures** and can be deployed from there. Open pull requests show
there as **in review**.

An architecture is something built *for* w8s: several clusters, each with a
part to play. A single upstream chart offered as it comes is the other half
of the catalog - **Apps → Vetted charts**, generated from a list rather
than contributed - and does not belong here.

## Contributing

The easiest way is in the app: **Apps → + Submit new**. It opens a pull
request here for you.

You can also open a pull request by hand. Add one directory:

```
architectures/
  your-architecture/
    architecture.yaml
    logo.svg          # optional, .svg or .png, at most 256 KiB
    files/            # optional: manifests, values, notes
```

The directory name is the architecture's id: lowercase letters, digits and
`-`. One architecture per pull request.

`architecture.yaml` (`architecture.example.yaml` in this repository is a
commented copy to start from):

```yaml
name: Redis with a replica per site
tagline: A primary in one cluster, a read replica in every other.
description: |
  Each cluster runs a Redis replica that follows the primary over the
  secure cluster connections. Reads stay local; writes go to the primary.
tags: [Redis, Primary/replica, ServiceExport, Helm]
namespace: cache
chart:
  url: oci://registry-1.docker.io/bitnamicharts/redis
  version: 20.1.0
source: https://github.com/bitnami/charts
author: Your name
logo: logo.svg
```

Only `name` is required. Unknown keys are rejected. An architecture that
runs across several clusters with different values per cluster adds a
`deploy:` section (roles, inputs, a values template); `couchdb/` and
`inference/` are examples to start from. Without `chart` the
architecture is listed but cannot be deployed from w8s until someone
packages it.

## Review

A maintainer checks that the chart installs on a plain conformant cluster,
that it needs nothing from a cloud provider, and that anything crossing
clusters goes through exported Services (`ServiceExport`), never a shared
network. Once merged, it appears in the app within a few minutes.

## Licence

Apache-2.0 (`LICENSE`). The charts our own architectures deploy are in
[w8s-cloud/charts](https://github.com/w8s-cloud/charts).
