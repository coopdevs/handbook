# Using prepared playbooks (recommended)

If you have access to the inventory of the odoo instance you want to restore, you can use the [restore playbook of odoo-provisioning](https://gitlab.com/coopdevs/odoo-provisioning#restoreyml). For more details, visit [restore playbook of backups-role](https://github.com/coopdevs/backups_role#playbook-to-restore-a-backup-to-the-controller).

Note that the `restore-to-controller` task list is intended to install restic and download it in the same machine where you execute ansible, i.e., the controller. If you want to restore it to the source server, you can use the `restore-to-host` task file to create a [simple playbook](https://github.com/coopdevs/backups_role#playbook-to-restore-a-backup-to-the-host)

# Without ansible

## Gather credentials for
  * Backblaze bucket
    * name
    * associated application key_id
    * associated application key
  * Restic repository
    * path within the bucket
    * encryption passphrase
  * Odoo DB master password

## Prepare restic

1. [Install restic](https://restic.readthedocs.io/en/latest/020_installation.html) on your workstation and check it executing `restic version`
2. Set environment variables:
```bash
export RESTIC_REPOSITORY='b2:BUCKET_NAME:REPO_PATH'
export RESTIC_PASSWORD='p4ssw0rD'
export B2_ACCOUNT_ID='MY_APPLICATION_KEY_ID'
export B2_ACCOUNT_KEY='MY_SECRET_ACCOUNT_KEY'
```
## Execute restic
1. Browse snapshots with `restic snapshots` and select which snapshot you want to restore.
2. Download snapshot file to your workstation with `restic restore $SNAPSHOT_ID .`
3. Check that the file is inside your working path inside a tree like './opt/backup/.tmp/odoo-backup.zip'.

## Odoo backup restore

WARNING, a better procedure must be studied, one that only deletes current db once the restore is checked. However, it involves database name and must be tested

1. Go to your Odoo instance db admin page looking like `https://my_odo.org/web/database/manager`
2. Delte your current db inputting db master password
3. Restore the backup by inputitng the previous database name, db master password, and selecting the zip to restore
4. Check results
  * Database manager webpage shows your db again with no warnings or errors
  * Log in to your odoo instance and check that you have restored the state you wished


## Other restic useful commands


#### View files inside the backup
The -l option gives the file size
```bash
restic  ls -l <snapshot> 
```
View the files inside the backup with then name <file>
```bash
restic  ls -l <snapshot>  <file>
```


#### Download one file
```bash
restic restore <snapshot> --target <backup destination folder> --include <file to download>
```
