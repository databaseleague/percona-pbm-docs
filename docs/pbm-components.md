# Percona Backup for MongoDB components

Percona Backup for MongoDB consists of the following components:


* [`pbm-agent`](reference/glossary.md#pbm-agent) is a process running on every `mongod` node within the cluster or a replica set that performs backup and restore operations.

* [PBM CLI](reference/glossary.md#pbm-cli) is a command-line utility that instructs pbm-agents to perform an operation.

    A single **pbm-agent** is only involved with one cluster (or non-sharded replica set). The `pbm` CLI utility can connect to any cluster it has network access to, so it is possible for one user to list and launch backups or restores on many clusters.


* [PBM Control collections](reference/glossary.md#pbm-control-collections) are special collections in MongoDB that store the configuration data and backup states. Both `pbm` CLI and **pbm-agent** use PBM Control collections to check backup status in MongoDB and communicate with each other.


* Remote backup storage is where Percona Backup for MongoDB saves backups. It can be either [an S3 compatible storage](reference/glossary.md#S3-compatible-storage) or a filesystem-type storage.

The following diagram illustrates how Percona Backup for MongoDB components communicate with MongoDB.

![image](_images/pbm-architecture.png){ align=center}

To learn more about Percona Backup for MongoDB architecture, see [Architecture](details/architecture.md).
