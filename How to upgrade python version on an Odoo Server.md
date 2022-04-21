# How to upgrade python version on an Odoo Server

In order to upgrade the python version it is necessary to remove the current version from the server.
You can find the installed python version in the following path ```cd /home/odoo/pyenv/versions/```.
Remove all the versions including *odoo*.

The odoo_release should also be removed since it depends on python's version.
It is under the path ```/opt/odoo_releases```.

Here the example commands, using -rf in order to force delete all the content of the folders:
```rm -rf 3.7.13/ odoo/`` 
```rm -rf odoo_12.0.12.0_2022-04-04.tar.gz```

This procedure should be made manually until we integrate it to odoo-provisioning.