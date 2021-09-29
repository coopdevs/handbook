## Intro

This is a guide of how to package Odoo addons, while our case study is Coopiteasy, it is perfectly valid for any Odoo addon.

[Coopiteasy](https://github.com/coopiteasy) developed a bunch of modules to easy the management of consumer cooperatives, the [_Vertical cooperative_ or _Easy my coop_](https://github.com/coopiteasy/vertical-cooperative).

They deploy these community modules directly from git, shallow-clonninig repos with git-aggregator, directly from within the host.

However, we use [[Ansible|Uso de Ansible en Coopdevs]] from working devices, and deploy community modules from pypi using pip and a requirements file.

As we are interested in their modules and want to keep the same deployment strategy we are using, we take charge on publishing them to pypi. The process right now is manual, but ideally would be triggered by commits to main branch.

We have already done this with some [non-odoo python works](https://gitlab.com/coopdevs/pyopencell/-/merge_requests/37)

## Sources

* [setuptools-odoo](https://pypi.org/project/setuptools-odoo/)
* [python guide: packaging and distributing](https://packaging.python.org/guides/distributing-packages-using-setuptools/)

## Process outline

1. Install needed tools
1. Create the `setup.py` with info from Odoo files
1. Build wheel package with `python setup.py ...`
1. Upload package(s) to pypi with `twine`

## Install tools

You can create and activate a venv:

```bash
python -m venv venv
source venv/bin/activate
```

And install the tools
```bash
pip install twine wheel setuptools-odoo
```

## Prepare a new repo for pypi

Assuming no previous work on pypi has been done, we need to check for the file structure. Odoo community has its own standards. It can be a single [repo module](https://pypi.org/project/setuptools-odoo/2.5.3/#packaging-a-single-addon), or more frequently, a [multi-module repo](https://pypi.org/project/setuptools-odoo/2.5.3/#packaging-multiple-addons). In any case, we will need a certain folder structure and a `__manifest__.py` for each addon.

### Some manifest data that caused problem in the past

* `version` must have 5 levels: 2 for Odoo (e.g. 12.0) and 3 for addon (e.g. 1.5.2)
* `website` must be a URL, therefore, must include the protocol part, i.e. `https://`
* `installable` is a flag to disable the addon from being installed to Odoo or being packed for pypi.

### Execute

Place yourself at the root of the repo:
```bash
ls .
addon1/
addon2/
```
And then run
```bash
setuptools-odoo-make-default -d .
```

### Expected results:
A new `setup/` folder with this structure:

```bash
...
setup/addon1/odoo/addons/<addon1_name> -> ../../../../<addon1_name>
setup/addon2/setup.py
...
```
This must be only once if no addons are added. Removed addons should remove too their setup/ symlinks.

### Commit the changes in setup/ directory
Since `setuptools-odoo` relies on what's committed to your Git repository, you need to commit the changes occurred in the `setup/` directory before executing the next steps.

If you don't do that, `setuptools` will create an empty `build/` directory and your package will be broken and not recognized by Odoo. More details [here](https://github.com/acsone/setuptools-odoo/issues/34#issuecomment-466789024).

## Build wheel

This is the first half of cyclical packaging process.
For each addon, do:
```bash
cd setup/addon_name
python setup.py bdist_wheel --universal sdist
cd ../..
```

### Checks
You can inspect the generated dir `setup/addon_name/odooXX_addon_addon_name.egg-info/`:
* `PKG-INFO`: version, name, author, etc. Mostly compiled from Odoo manifest file. See [how and why weird version numbers like `···99.dev17` are computed](https://pypi.org/project/setuptools-odoo/2.5.3/#versioning)
* `SOURCES.txt`: included files. Useful if you added a new translation or class and want to ensure it's there. Bear in 
* `requires.txt`: pip dependencies.

This whole process is git-dependand. Both versions and sources depend on git: version to decide the devXX number, and sources to ignore untracked files.

For any doubt of what's inside, you can unzip the `whl` file in `dist/` to see what's in.

## Upload to pypi

If you are testing, you can use the [test pypi instance](https://test.pypi.org/) just by inserting `--repository-url https://test.pypi.org/legacy/` to the next command, just after `upload`:

Otherwise, just do for each addon to upload:
```bash
cd setup/addon_name
twine upload dist/*
# fill in credentials
# wait for the upload
cd ../..
```

### Checks
* visit the web repo linked by twine output, like the one for [easy-my-coop-website](https://pypi.org/project/odoo12-addon-easy-my-coop-website/12.0.1.0.0.99.dev23/)
* install with pip. E.g. `pip install odoo12-addon-easy_my_coop_website==12.0.1.0.0.99.dev23`. If testing, just insert `--index-url https://test.pypi.org/simple/` after `install`.