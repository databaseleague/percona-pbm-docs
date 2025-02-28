# Download Percona Backup for MongoDB from Percona website

You can download Percona Backup for MongoDB from [Percona website](https://www.percona.com/downloads/percona-backup-mongodb/) and install it:

* [From binary tarballs](#install-from-binary-tarball).
* Manually, from the installation packages using `dpkg` (Debian and Ubuntu) or `rpm` (Red Hat Enterprise Linux and CentOS). However, you must make sure that all dependencies are satisfied.


## Install from binary tarball

Find the link to the binary tarballs under the **Generic Linux** menu item on [Percona website](https://www.percona.com/downloads/percona-backup-mongodb/).


### 1. Fetch the binary tarball

Replace the `<version>` with the required version.

   ```{.bash data-prompt="$"}
   $ wget https://downloads.percona.com/downloads/percona-backup-mongodb/percona-backup-mongodb-<version>/binary/tarball/percona-backup-mongodb-<version>-x86_64.tar.gz
   ```


### 2. Extract the tarball

  ```{.bash data-prompt="$"}
  $ tar -xf percona-backup-mongodb-<version>-x86_64.tar.gz
  ```

### 3. Export the location of the binaries to the `PATH` variable

For example, if you’ve extracted the tarball to your `home` directory, the command would be the following:

  ```{.bash data-prompt="$"}
  $ export PATH=~/percona-backup-mongodb-<version>/:$PATH
  ```

After Percona Backup for MongoDB is successfully installed on your system, you have `pbm` and `pbm-agent` programs available. See [Initial setup](initial-setup.md) for guidelines how to set up Percona Backup for MongoDB.