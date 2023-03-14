# Install Percona Backup for MongoDB

## Supported platforms

Percona Backup for MongoDB is available for the most 64-bit Linux distributions. Find the list of supported platforms on the [Percona Software and Platform Lifecycle](https://www.percona.com/services/policies/percona-software-platform-lifecycle#mongodb) page.

## What nodes to install on

### `pbm-agent`

Install [`pbm-agent`](details/pbm-agent.md) on all servers that have `mongod` nodes in the
MongoDB cluster (or non-sharded replica set). You don't need to start it on the `arbiter` node, since it doesn’t have the data set.

### `pbm` CLI

You can install [`pbm` CLI](details/cli.md) on any or all servers or desktop computers you wish to use it from. Those computers must not be network-blocked from accessing the MongoDB cluster.

## Installation tutorials

Install Percona Backup for MongoDB in one of the following ways:

* [from Percona repositories](install/repos.md) using the package manager of your operating system. This is the recommended way

* [build from source code](install/source.md) if you want full control over the installation

* [download tarballs from Percona website](install/tarball.md) 

* [run Percona Backup for MongoDB in a Docker container](https://hub.docker.com/r/percona/percona-backup-mongodb)

* [deploy Percona Backup for MongoDB on Kubernetes](install/cloud.md)

After the installation completes, you have the following tools:

| Tool            | Purpose                                                  |
| --------------- | ---------------------------------------------------------|
| `pbm`           | Command-line interface for controlling the backup system |
| `pbm-agent`     | An agent for running backup/restore actions on a database host |
| `pbm-speed-test`| An interface for field-testing compression and backup upload speed|
| `pbm-agent-entrypoint` | An entry point application that allows starting `pbm-agent` and also restarts it in case of any faults| 


## Next steps

[Install from Percona repositories](install/repos.md)


