# wordpress-backup

Script to backup, restore and migrate Wordpress data. 

This backup script is designed to work with WordPress installed using the script at https://github.com/basilhendroff/freenas-iocage-wordpress

## Status
This script will work with FreeNAS 11.3, and it should also work with TrueNAS CORE 12.0.  Due to the EOL status of FreeBSD 11.2, it is unlikely to work reliably with earlier releases of FreeNAS.

## Usage

### Prerequisites
You must have a working Wordpress install to use this script. Although not required, it's recommended to create a Dataset named `backup` on your main storage pool. If this dataset is not present, the directory `/backup` and will be created in `$POOL_PATH`
.
### Installation
Download the repository to a convenient directory on your FreeNAS system by changing to that directory and running `git clone https://github.com/basilhendroff/wordpress-backup`.  Then change into the new `wordpress-backup` directory.

### Setup
If you've used the previously mentioned install script and accepted the default WordPress jail name `wordpress`, additional setup isn't necessary for the backup script to run. If you have changed the jail name, or have installed WordPress using another method, then create a file called `backup-config` with your favorite text editor.  In its minimal form, it would look something like this:

```
JAIL_NAME="wp-blog"
```
The backup script options have sensible defaults, which can be adjusted if needed. These are:

- JAIL_NAME: The name of the WordPress jail, defaults to `wordpress`.
- BACKUP_PATH: Backups are stored in this location. Default is the directory `/backup` under the pool path.
- TARGET_JAIL: This parameter is only invoked when migrating a WordPress site from one jail to another.

Some examples follow:

#### 1. *'I'm using the default jail name of `wordpress`.'*
A backup-config is not required.

#### 2. *'I'm using the WordPress jail name `personal`.'*
backup-config:
```
JAIL_NAME="personal"
```
Note: Be aware that the jail name is case sensitive.

#### 3. *`I also want my backups stored elsewhere.'*
```
JAIL_NAME="personal"
BACKUP_PATH="/mnt/tank/cloud"
```

#### 4. *`I want to migrate my WordPress site to a another jail.'*
```
JAIL_NAME="source"
TARGET_JAIL="destination"
```

### Execution
Once you've prepared the configuration file (if required), run the script `script backup.log ./backup-jail.sh`.

### Backup
When run, choose backup when prompted to (B)ackup or (R)estore. 

To automate backup, create a cron job pointing to the backup script.

### Restore
**WARNING: A restore overwrites existing WordPress data on the target!!!**

When run, choose restore when prompted to (B)ackup or (R)estore. You will be prompted once more to reconfirm. The default action is to abort.

### Migrate
**WARNING: A migrate overwrites existing WordPress data on the target!!!**

When run, you will be prompted to confirm the migration. The default action is to abort.

## Support and Discussion
Reference: [WordPress Backups](https://wordpress.org/support/article/wordpress-backups/)

Questions or issues about this resource can be raised in [this forum thread](). Support is limited to getting the backup script working with your WordPress jail. 

## Disclaimer
It's your data. It's your responsibility. This resource is provided as a community service. Use it at your own risk.
