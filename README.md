# Telemetry Development Environment

Since Telemetry gets its data from Rancher and Rancher requires a Kubernetes
Cluster to run (and to be able to provide any telemetry data), the development
environment spins up a local Kubernetes Cluster. It does so using
[k3d](https://k3d.io/v5.4.6/).

> k3d is a lightweight wrapper to run k3s (Rancher Lab’s minimal Kubernetes distribution) in docker.
>
> k3d makes it very easy to create single- and multi-node k3s clusters in docker, e.g. for local development on Kubernetes.

## Requirements

- docker
- k3d

## Quick Start

```console
./start
```

Then visit https://rancher/.

The username is `admin`, the password is `dev-admin-pw`.

## What's being deployed?

By running `start`, a bunch of things happen:

- A k3d cluster is deployed
- Rancher and dependencies are installed
- The hostname `rancher` is configured
- A login token is created and retrieved
- The login token is used to create an API token
- The Rancher server URL is configured

> **_NOTE_**: The development cluster is destroyed and recreated without
> requiring a confirmation if `start` is rerun.

(Optionally) install

- Telemetry Server
- PostgreSQL
- Telemetry Client

from the provided Helm charts in the `charts` directory.

You can do so by running

```console
helm install postgres charts/postgresql
helm install client charts/rancher-telemetry-client
helm install server charts/rancher-telemetry-server
```

## Can I run some steps manually?

Yes! The way to do that is by sourcing the `start` script. Then all the
functions defined in that script, that are executed if the script is run, can be
called manually.

```bash
source start
# then, for example, run
postgres-install
# or
configure-hostnames
# or 
api_token=$(create-api-key $(get-login-token))
```

> **_NOTE_**: If you don't run `change-password`, the bootstrap password will still be `admin` and you will be shown the setup-screen.
