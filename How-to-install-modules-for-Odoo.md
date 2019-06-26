## Steps overview:

1. Install the python package for the module to the odoo virtual env
2. Update Odoo's app list
3. Install internally the module or "app"

Correcte, els moduls que no estan en pip els podem instalar amb pip també des de git, pero aquesta part jo encara no l'he testejat

https://pip.pypa.io/en/stable/reference/pip_install/#vcs-support

## Enter the virtual env

```sh
user@host:/opt$ sudo su odoo
odoo@host:/opt$ source .odoo_venv/bin/activate
(.odoo_venv) odoo@host:/opt$ 
```


## Install via `pip` the python package for the module.  
The name will be on the form of `odoo<VERSION>-addon-<ADDON-NAME>`, as in `odoo11-addon-project-key`.

### To install from pyPI

```
pip install odoo11-addon-project-key
```

### To install from a git repo

To be investigated. pip supports [installing packages served by git+transports](https://pip.pypa.io/en/stable/reference/pip_install/#vcs-support), but it may be trickier that it looks. It could be as easy as:

```sh
(.odoo_venv) odoo@host:/opt$ pip install -e git+https://github.com/OCA/project.git/#egg=project_key&subdirectory=/project_key

```

## Update Odoo app list

Odoo needs to refresh and detect all addons that are available on its virtual env.

### via cli
To do it via command line, the kind of official way to do it is to "update" any installed module. This makes Odoo to look for undetected modules.
```sh
(.odoo_venv) odoo@host:~$ python /path/to/odoo -c /path/to/odoo.conf -d "odoo_db_name" --update base --stop-after-init --without-demo=all
```

### via web UI

1. Go to [Apps](https://your-odoo-site.org/web#view_type=kanban&model=ir.module.module)
2. Click on [Update Apps List](https://your-odoo-site.org/web#menu_id=48&action=35)
3. Click "Update"

## Install Odoo app

### via cli

```sh
(.odoo_venv) odoo@host:~$ python /path/to/odoo -c /path/to/odoo.conf -d "odoo_db_name" --init "project-key" --stop-after-init --without-demo=all
```

### via web UI

1. Go to [Apps](https://your-odoo-site.org/web#view_type=kanban&model=ir.module.module)
2. Find your addon (maybe disable "Apps" filter)
3. Click "Install" on the addon box.


---

## Sources

* [Odoo role tasks](https://github.com/coopdevs/odoo-role/blob/master/tasks/community-modules.yml)
* [pip reference](https://pip.pypa.io/en/stable/reference/pip_install/#vcs-support)