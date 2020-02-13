## Intro

[Coopiteasy](https://github.com/coopiteasy) developed a bunch of modules to easy the management of consumer cooperatives, the [_Vertical cooperative_ or _Easy my coop_](https://github.com/coopiteasy/vertical-cooperative).

They deploy these community modules directly from git, shallow-clonninig repos with git-aggregator, directly from within the host.

However, we use [[Ansible|Uso de Ansible en Coopdevs]] from working devices, and deploy community modules from pypi using pip and a requirements file.

As we are interested in their modules and want to keep the same deployment strategy we are using, we take charge on publishing them to pypi. The process right now is manual, but ideally would be triggered by commits to main branch.

We have already done this with some [non-odoo python works](https://gitlab.com/coopdevs/pyopencell/-/merge_requests/37)

## Sources

* [setuptools-odoo](https://pypi.org/project/setuptools-odoo/)
* [python guide: packaging and distributing](https://packaging.python.org/guides/distributing-packages-using-setuptools/)

## Process outline

In reverse order:

1. upload package(s) to pypi with `twine`
2. build wheel package with `python setup.py ...`
3. create the `setup.py` with info from odoo files
4. install needed tools

## Install tools

You can create and activate a venv:

```shell
python -m venv venv
source venv/bin/activate
```

And install the tools
```
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
```shell
ls .
addon1/
addon2/
```
And then run
```shell
setuptools-odoo-make-default -d .
```

### Expected results:
A new `setup/` folder with this structure:

```
...
setup/addon1/odoo/addons/<addon1_name> -> ../../../../<addon1_name>
setup/addon2/setup.py
...
```
This must be only once if no addons are added. Removed addons should remove too their setup/ symlinks.

## Build wheel

This is the first half of cyclical packaging process.
For each addon, do:
```
cd setup/addon_name
python setup.py bdist_wheel --universal sdist
cd ../..
```

### Checks
You can inspect the generated dir `setup/addon_name/odooXX_addon_addon_name.egg-info/`:
* PKG-INFO: version, name, author, etc. Mostly compiled from Odoo manifest file. See [how and why weird version numbers like `···99.dev17` are computed](https://pypi.org/project/setuptools-odoo/2.5.3/#versioning)
* requires.txt: pip dependencies.
* SOURCES.txt: included files. Useful if you added a new translation or class and want to ensure it's there. Bear in 

This whole process is git-dependand. Both versions and sources depend on git: version to decide the devXX number, and sources to ignore untracked files.

For any doubt of what's inside, you can unzip the `whl` file in `dist/` to see what's in.

## Upload to pypi

If you are testing, you can use the [test pypi instance](https://test.pypi.org/) just by inserting `--repository-url https://test.pypi.org/legacy/` to the next command, before `dist`:

For each addon to upload, do:
```bash
cd setup/addon_name
twine upload dist/*
# fill in credentials
# wait for the upload
cd ../..
```