## Process for Odoo 12 and 13

This is also described at the "module info" webpage of "Base Location Geonames Import" and the module's [README](https://github.com/OCA/partner-contact/blob/12.0/base_location_geonames_import/README.rst#usage)

1. Verify Odoo [timeout settings](https://github.com/coopdevs/handbook/wiki/How-to-import-postal-codes-for-a-country#in-case-of-a-timeout-or-a-server-restart)
1. Go to Contacts > Configuration > Localization > Import from Geonames, and click on it to open a wizard.
1. When you start the wizard, it will ask you to select a country.
1. Then, for the selected country, it will delete all not detected entries, download the latest version of the list of cities from geonames.org and create new city zip entries.

## Process for Odoo 11

The place to launch the import is different. It's described also at the "module info" webpage and [README](https://github.com/OCA/partner-contact/blob/11.0/base_location_geonames_import/README.rst#usage)

1. Verify Odoo [timeout settings](https://github.com/coopdevs/handbook/wiki/How-to-import-postal-codes-for-a-country#in-case-of-a-timeout-or-a-server-restart)
1. Go to Settings > Technical > Cities/Locations Management > Import from Geonames, and click on it to open a wizard.
1. Follow step 2 and 3 for Odoo 12 and 13

## Troubleshooting

### In case of a timeout or a server restart

If the import process takes too long, Odoo [may kill itself the worker thread and leave the task undone](https://github.com/OCA/partner-contact/issues/829). To avoid this, you can increase the thread timeout, perform the action, and set it default or restore it when finished.

1. Add this to your odoo.config:
   ```
   ; Increase timeout of thread worker from 2 to 10 minutes
   limit_time_real = 6000
   ```
2. Restart Odoo
3. Perform import as specified above
4. Reset the timeout to the default value to avoid side effects.
5. Restart Odoo

### In case of 404 error

Sometimes Geonames [has issues with its releases](https://groups.google.com/forum/#!topic/geonames/HyAZeqZe3K4), and the release stays void for a whole day or so. If this happens and we can't wait until they fix it, we can change instead the download URL and use a backup copy from the Internet Archive.

1. Go to Configuració/Settings > Paràmetres del Sistema/System Parameters
2. Add or modify an entry with:
  - Key: geonames.url
  - Value: https://web.archive.org/web/20191029012851/https://download.geonames.org/export/zip/%s.zip