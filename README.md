# Telemetry Development Environment

Since Telemetry gets its data from Rancher and Rancher requires a Kubernetes
Cluster to run (and to be able to provide any telemetry data), the development
environment spins up a local Kubernetes Cluster. It does so using
[k3d](https://k3d.io).

> k3d is a lightweight wrapper to run k3s (Rancher Lab’s minimal Kubernetes
> distribution) in docker.
>
> k3d makes it very easy to create single- and multi-node k3s clusters in
> docker, e.g. for local development on Kubernetes.

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
- Postgresql is installed
- InfluxDB is installed

> **_NOTE_**: The development cluster is destroyed and recreated without
> requiring a confirmation if `start` is rerun.

## Can I just install the base system?

Yes! The base system is defined as k3d with Rancher. What's not installed is
PostgreSQL or InfluxDB.

To install the base system, you can source `start` and run `install-base`:

```bash
source start && install-base
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

> **_NOTE_**: If you don't run `change-password`, the bootstrap password will
> still be `admin` and you will be shown the setup-screen.

## I want data to be imported!

PostgreSQL data is persisted on the host system in `/var/lib/postgresql/data`.
The script `start` ensures it is created and available. It also makes it
available to the k3d nodes, so that it can be used as volume for containers
created on that nodes.

SQL instructions can be placed as files in
`/var/lib/postgresql/docker-entrypoint-initdb.d`. They will be executed on first
start of the PostgreSQL container. If the database has been initialized once,
the SQL files in that folder will be ignored.
