# bash script to backup a linux machine using aws

The script assumes an existing aws account and aws cli tools have been installed on the client. The aws cli's credentials must have been correctly configured and command output set to 'text'. The script only uses aws CPU and disk space while in operation to minimize $$. The script backups incremental changes from the last backup and assumes that a full backup was done to start with.

This script generally follows these steps (with error checking).
1. Get external IP address of host (also serves to check for Internet connection.)
2. Create a volume from the previous backup snapshot.
3. Create an ec2 security group. 
4. Create an machine instance with ec2.
5. Attach a volume to it.
6. Mount the volume.
7. rsync some directories to the volume mounted on the remote machine
8. Unmount and detach the volume.
9. Terminate the machine.
10. Delete security group.
11. Backup the volume to an S3 snapshot. 
12. Delete the volume.

Note that ServerAliveInterval and ServerAliveCountMax should be set with non-default values in your SSH config to avoid SSH timeouts when backing up large datasets. See [this](https://unix.stackexchange.com/questions/3026/what-options-serveraliveinterval-and-clientaliveinterval-in-sshd-config-exac) for details. 
