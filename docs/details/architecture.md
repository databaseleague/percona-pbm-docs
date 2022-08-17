# Architecture

## **pbm-agent**

A `pbm-agent` is a process that runs backup, restore, delete and other operations available with Percona Backup for MongoDB.

A `pbm-agent` instance must run on each `mongod` instance. This includes replica set nodes that are currently secondaries and config server replica set nodes in a sharded cluster.

An operation is triggered when the `pbm` CLI makes an update to the [PBM Control collection](#pbm-control-collections). All `pbm-agents` monitor changes to the PBM control collections but only one `pbm-agent` in each replica set will be elected to execute an operation. The elections are done by a random choice among secondary nodes. If no secondary nodes respond, then the `pbm-agent` on the primary node is elected for an operation.

The elected `pbm-agent` acquires a lock for an operation. This prevents mutually exclusive operations like backup and restore to be executed simultaneously.

When the operation is complete, the `pbm-agent` releases the lock and updates the PBM control collections.

## PBM Command Line Utility (`pbm`)

`pbm` CLI is the command line tool with which you operate Percona Backup for MongoDB. `pbm` provides the **pbm** command that you will use manually in the shell. It will also
work as a command that can be executed in scripts (for example, by `crond`).

The set of [pbm sub-commands](../reference/pbm-commands.md) enables you to manage backups in your MongoDB environment.

`pbm` uses [PBM Control collections](#pbm-control-collections) to communicate with `pbm-agent` processes. It starts and monitors backup or restore operations by updating and reading the corresponding PBM control collections for operations, log, etc. Likewise, it modifies the PBM config by saving it in the PBM Control collection for config values.

`pbm` does not have its own config and/or cache files. Setting the
`PBM_MONGODB_URI` environment variable in your shell is a
configuration-like step that should be done for practical ease though. (Without
`PBM_MONGODB_URI` the `--mongodb-uri` command line argument will need to
be specified each time.)

To learn how to set the `PBM_MONGODB_URI` environment variable, see [Set the MongoDB connection URI for `pbm` CLI](../initial-setup.md#set-mongodburi-pbm-cli). For more information about MongoDB URI connection strings, see [Authentication](authentication.md).

## PBM Control Collections

The config and state (current and historical) for backups is stored in
collections in the MongoDB cluster or non-sharded replica set itself. These are
put in the system `admin` db to keep them cleanly separated from user db namespaces.

In sharded clusters, this is the `admin` db of the config server replica set. In a non-sharded replica set, the PBM Control Collections are stored in
`admin` db of the replica set itself.

* *admin.pbmBackups* - Log / status of each backup
* *admin.pbmAgents* - Contains information about `pbm-agents` statuses and health
* *admin.pbmConfig* - Contains configuration information for Percona Backup for MongoDB
* *admin.pbmCmd* - Is used to define and trigger operations
* *admin.pbmLock* - **pbm-agent** synchronization-lock structure
* *admin.pbmLockOp* - Is used to coordinate operations that are not mutually-exclusive such as make backup and delete backup.
* *admin.pbmLog* - Stores log information from all `pbm-agents` in the MongoDB environment. Available in Percona Backup for MongoDB as of version 1.4.0
* *admin.pbmOpLog* - Stores [operation IDs](../reference/glossary.md#opids)
* *admin.pbmPITRChunks* - Stores [Point-in-Time Recovery](../reference/glossary.md#point-in-time-recovery) oplog slices
* *admin.pbmPITRState* - Contains current state of Point-in-Time Recovery incremental backups
* *admin.pbmRestores* - Contains restore history and the restore state for all replica sets
* *admin.pbmStatus* - Stores Percona Backup for MongoDB status records

The `pbm` command line tool creates these collections as needed. You do not
have to maintain these collections, but you should not drop them unnecessarily
either. Dropping them during a backup will cause an abort of the backup.

Filling the config collection is a prerequisite to using Percona Backup for MongoDB for executing backups or restores. (See config page later.)

## Remote backup storage

Percona Backup for MongoDB saves your files to a directory. Conceptually in
the case of object store; actually if you are using filesystem-type remote
storage. Using **pbm list**, a user can scan this directory to find existing
backups even if they never used `pbm` on their computer before.

The files are prefixed with the (UTC) starting time of the backup. For each
backup there is one metadata file. For each replica set, a backup includes the following:


* A mongodump-format compressed archive that is the dump of collections
* A (compressed) BSON file dump of the oplog covering the timespan of the backup.

The end time of the oplog slice(s) is the data-consistent point in time of a backup snapshot.

For details about supported backup storages, see [Remote backup storage](storage-configuration.md).
