#### Run tests

You can run the tests of your custom module with this command:
```sh
$ ./odoo-bin -c /etc/odoo/odoo.conf -u <module> -d odoo --stop-after-init --test-enable --workers 0
```
If you aren't using a multi worker Odoo, you can avoid using the option `--workers 0`.

#### Run tests with coverage

You can run the tests with a coverage report following the nexts steps:

1. Copy the [.coveragerc](https://github.com/coopdevs/maintainer-quality-tools/blob/master/cfg/.coveragerc) file in your `odoo` base path (`/opt/odoo`) changing the `include` option to the `somconnexio` module path (`/opt/odoo_modules/somconnexio/*`).
2. Go to `/opt/odoo`
3. Run:
```sh
$ coverage run odoo-bin -c /etc/odoo/odoo.conf -u <module> -d odoo --stop-after-init --test-enable --workers 0 && coverage report --show-missing
```

***

# Configure CI

We are using the [MQT](https://github.com/odoo-dominicana/maintainer-quality-tools) to run the tests in the Gitlab CI.

In the `sample_files` config you can find an example ob `gitlab-ci.yml` to configure in your custom module:

https://github.com/odoo-dominicana/maintainer-quality-tools/blob/master/sample_files/.gitlab-ci.yml


***


# [`pytest-odoo`](https://github.com/camptocamp/pytest-odoo) within [devenv](https://github.com/coopdevs/handbook/wiki/Devenv)


1. *ssh* into devenv

```bash
ssh odoo@odoo.local
```

2. activate virtual enviroment
```bash
pyenv activate odoo
```

3. install dependencies

```bash
pip install pytest==7.1.1 pytest-odoo==0.7.1 pytest-html==3.1.1 pytest-metadata==2.0.1 coverage==6.3.2
```

4. run pytest with `--odoo-config` and `--odoo-database` parameters
```bash
pytest -s --odoo-config=/etc/odoo/odoo.conf --odoo-database=odoo -p no:warnings /opt/odoo_modules
```


```
platform linux -- Python 3.7.7, pytest-7.1.1, pluggy-1.0.0
rootdir: /opt
plugins: metadata-2.0.1, odoo-0.7.1, html-3.1.1
collected 3 items

../odoo_modules/sample_module/tests/test_collection.

================================== 3 passed in 0.25s ====================================

```

### Generate an HTML coverage report

1.  make directory
```bash
mkdir /home/odoo/coverage
```

2. add `--html=` parameter to `pytest` command.
```bash
pytest -s --odoo-config=/etc/odoo/odoo.conf --odoo-database=odoo -p no:warnings --html=/home/odoo/coverage/report.html /opt/odoo_modules
```
