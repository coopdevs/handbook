## Uninstall account\_budget from v11
It is not in v12 and it makes problems

## Update module list from v11 to v12
This is an example from odoo-coopdevs-treball-inventory

requirements.txt:

```
odoo12-addon-account-banking-mandate==12.0.2.0.2
odoo12-addon-account-banking-pain-base==12.0.1.0.2
odoo12-addon-account-banking-sepa-credit-transfer==12.0.1.0.0.99.dev7
odoo12-addon-account-banking-sepa-direct-debit==12.0.1.3.0
odoo12-addon-account-due-list==12.0.1.0.0.99.dev8
odoo12-addon-account-financial-report==12.0.1.2.2.99.dev31
odoo12-addon-account-multicompany-easy-creation==12.0.1.0.0.99.dev3
odoo12-addon-account-payment-mode==12.0.1.0.1
odoo12-addon-account-payment-order==12.0.1.6.0
odoo12-addon-account-payment-partner==12.0.1.0.0.99.dev15
odoo12-addon-account-tax-balance==12.0.1.1.1.99.dev6
odoo12-addon-base-bank-from-iban==12.0.1.0.0.99.dev2
odoo12-addon-base-location==12.0.1.0.3
odoo12-addon-base-location-geonames-import==12.0.1.0.4
odoo12-addon-base-technical-features==12.0.1.1.0.99.dev3
odoo12-addon-contract==12.0.7.2.0
odoo12-addon-contract-sale-invoicing==12.0.1.0.2.99.dev6
odoo12-addon-contract-variable-qty-timesheet==12.0.1.0.0.99.dev7
odoo12-addon-contract-variable-quantity==12.0.3.0.0
odoo12-addon-date-range==12.0.1.0.0.99.dev16
odoo12-addon-l10n-es-account-asset==12.0.2.0.2
odoo12-addon-l10n-es-account-bank-statement-import-n43==12.0.1.0.4
odoo12-addon-l10n-es-account-invoice-sequence==12.0.1.0.2
odoo12-addon-l10n-es-aeat==12.0.2.2.0
odoo12-addon-l10n-es-aeat-mod111==12.0.1.3.0
odoo12-addon-l10n-es-aeat-mod115==12.0.1.4.0
odoo12-addon-l10n-es-aeat-mod303==12.0.1.8.0
odoo12-addon-l10n-es-aeat-mod349==12.0.1.3.0
odoo12-addon-l10n-es-aeat-mod390==12.0.2.4.0
odoo12-addon-l10n-es-mis-report==12.0.1.0.0.99.dev5
odoo12-addon-l10n-es-partner==12.0.1.0.2
odoo12-addon-l10n-es-toponyms==12.0.1.0.0.99.dev8
odoo12-addon-l10n-es-vat-book==12.0.1.4.3
odoo12-addon-mail-tracking==12.0.2.1.3
odoo12-addon-mail-tracking-mailgun==12.0.1.0.2
odoo12-addon-mass-editing==12.0.1.1.0
odoo12-addon-mis-builder==12.0.3.6.2
odoo12-addon-mis-builder-budget==12.0.3.5.0
odoo12-addon-password-security==12.0.1.1.2
odoo12-addon-project-task-default-stage==12.0.1.0.0.99.dev14
odoo12-addon-report-xlsx==12.0.1.0.3
odoo12-addon-sale-order-invoicing-finished-task==12.0.1.0.0.99.dev6
odoo12-addon-sale-order-line-input==12.0.1.0.0.99.dev8
odoo12-addon-web-decimal-numpad-dot==12.0.1.0.0.99.dev5
odoo12-addon-web-favicon==12.0.1.0.0.99.dev12
odoo12-addon-web-no-bubble==12.0.1.0.0.99.dev5
odoo12-addon-web-responsive==12.0.2.3.0
odoo12-addon-web-searchbar-full-width==12.0.1.0.0.99.dev3
odoo12-addon-web-widget-color==12.0.1.0.0.99.dev5
```
## Clone OpenUpgrade repo using depth=1 to save half an hour downloading commits 
```sh
git clone --depth 1 --branch 12.0 https://github.com/OCA/OpenUpgrade.git
```

## Create a virtualenv
Minimum Python version 3.5

## Install OpenUpgrade in new virtualenv
```
python setup.py install
```

## Install Odoo modules in Openupgrade virtualenv
```sh
/opt/odoo_modules_v12 $ pip install -r requirements.txt 
```

## Create /etc/odoo/odoo_upg_11_12.conf
```
[options]
; Log Settings
logfile = /var/log/odoo/odoo-upg.log
log_level = debug

; Custom Modules
addons_path = /opt/OpenUpgrade/addons,/opt/odoo_modules_v12

; Master password to manage dbs
admin_passwd = 1234

; HTTP server settings
http_interface = 0.0.0.0
proxy_mode = True
; prod es el nombre del backup de odoo v11 en producción actual
db_name = prod
; Postgresql 9.6 port (9.5=5432)
db_port = 5433
; Allow to select another (not filtered by dbfilter) existing database and enable db manager
list_db = False
```
## Install Postgresql 9.6 from Ubuntu repos 
`https://www.postgresql.org/download/linux/ubuntu/`
postgresql >= 9.6 is needed by upgrade scripts

## Change Odoo v11 Postgresql port
Using Postgresql 9.6 db directly in restore phase makes the DB upgrade to 9.6 unnecessary. NOT SURE
```
db_port = 5433
```

## Restore DB backup of v11 that it is going to be updated
From `/web/database/manager`

## Launch Odoo migration
Using the db name we have just restored. From the dir where the OpenUpgrade repo is located.
```
odoo@odoo-coopdevs:/opt/OpenUpgrade$ ./odoo-bin -c /etc/odoo/odoo_upg_11_12.conf --stop-after-init -d db_name --update all
```

## Uncompress selected OCA/OCB Odoo version
In `/opt/odoo_12`

## Create the v12 definitive conf file
In `/etc/odoo/odoo_12.conf`
```
[options]
; Log Settings
logfile = /var/log/odoo/odoo-12.log
log_level = debug

; Custom Modules
addons_path = /opt/odoo_12/addons,/opt/odoo_modules_v12

; Master password to manage dbs
admin_passwd = 1234

; HTTP server settings
http_interface = 0.0.0.0
proxy_mode = True

db_name = [[ db_name ]]
db_port = 5433
; Allow to select another (not filtered by dbfilter) existing database and enable db manager
list_db = True
```
## Update all modules with OCB v12 version

```sh
odoo@odoo-coopdevs:/opt/odoo_12$ ./odoo-bin -c /etc/odoo/odoo_12.conf --stop-after-init -d prod --update all
```

## Use pg_dump 9.6 to export migrated db
```
/opt/odoo_12/odoo/service/db.py:L212:
cmd = ['/usr/lib/postgresql/9.6/bin/pg_dump', '--no-owner']
```

## Export db from db manager
Use url `/web/database/manager`

---

## Sources

* [Open Upgrade doc](https://doc.therp.nl/openupgrade/)