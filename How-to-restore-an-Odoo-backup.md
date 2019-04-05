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

1. [Install restic](https://restic.readthedocs.io/en/latest/020_installation.html) on your workstation and check it executing `restic --version`
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

