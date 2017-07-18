---
title: docker/ucp restore
description: Restore a UCP cluster from a backup
keywords: docker, ucp, cli, restore
---

Restore a UCP cluster from a backup

## Usage

```
docker run --rm -i \
        --name ucp \
        -v /var/run/docker.sock:/var/run/docker.sock \
        docker/ucp \
        restore [command options] < backup.tar
```

## Description

This command installs a new UCP cluster that is populated with the state of
a previous UCP manager node using a tar file generated by the 'backup' command.
All UCP settings, users, teams and permissions will be restored from the backup
file. The Restore operation does not alter or recover any containers, networks,
volumes or services of an underlying swarm.

The restore command can be performed on any manager node of an existing
swarm. If the current node does not belong in a swarm, one will be
initialized using the value of the '--host-address' flag. When restoring on an
existing swarm-mode cluster, no previous UCP components must be running on any
node of the cluster. This cleanup can be performed with the 'uninstall-ucp'
command.

If restore is performed on a different swarm than the one
where the backup file was taken on, the Cluster Root CA of the old UCP
installation will not be restored. This will invalidate any
previously issued Admin Client Bundles and all administrator will be required
to download new client bundles after the operation is completed.
Any existing Client Bundles for non-admin users will still be fully
operational.

By default the backup tar file is read from stdin. You can also bind-mount the
backup file under /config/backup.tar, and run the restore command with the
'--interactive' flag.

Notes:

  * Please run 'uninstall-ucp' before attempting the restore operation on an
    existing UCP cluster.

  * If your swarm-mode cluster has lost quorum and the original set of managers
    are not recoverable, you can attempt to recover a single-manager cluster
  with 'docker swarm init --force-new-cluster'.

  * You can restore from a backup that was taken on a different manager node or
    a different swarm altogether.


## Options

| Option                    | Description                |
|:--------------------------|:---------------------------|
|`--debug, D`|Enable debug mode|
|`--jsonlog`|Produce json formatted output for easier parsing|
|`--interactive, i`|Run in interactive mode and prompt for configuration values|
|`--passphrase`|Decrypt the backup tar file with the provided passphrase|
|`--san`|Add subject alternative names to certificates (e.g. --san www1.acme.com --san www2.acme.com)|
|`--host-address`|The network address to advertise to other nodes. Format: IP address or network interface name|
|`--unlock-key`|The unlock key for this swarm-mode cluster, if one exists.|