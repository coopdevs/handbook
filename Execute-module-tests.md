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

### Run with coverage

```bash
coverage run --source=/opt/odoo_modules/ -m pytest -s  --odoo-config=/etc/odoo/odoo.conf --odoo-database=odoo -p no:warnings  /opt/odoo_modules/ && coverage report -m
```

```
============================================================================ test session starts =============================================================================
platform linux -- Python 3.7.7, pytest-7.1.1, pluggy-1.0.0
rootdir: /opt/odoo_modules
plugins: metadata-2.0.1, odoo-0.7.1, html-3.1.1
collected 3 items                                                                                                                                                            

../../opt/odoo_modules/sample_module/tests/test_collection.py ...

============================================================================= 3 passed in 0.32s ==============================================================================
Name                                                                Stmts   Miss  Cover   Missing
-------------------------------------------------------------------------------------------------
/opt/odoo_modules/sample_module/__init__.py                                  4      4     0%   3-6
/opt/odoo_modules/sample_module/__manifest__.py                              1      1     0%   4
/opt/odoo_modules/sample_module/controllers/__init__.py                      1      0   100%
/opt/odoo_modules/sample_module/controllers/main.py                         31     19    39%   13-14, 25, 32-64
/opt/odoo_modules/sample_module/models/__init__.py                           2      0   100%
/opt/odoo_modules/sample_module/models/dav_collection.py                   147     35    76%   63-67, 71-73, 98, 118, 120, 122-123, 129, 147-148, 175-194, 202, 207, 219, 231, 237, 259-276
/opt/odoo_modules/sample_module/models/dav_collection_field_mapping.py      86     16    81%   96-98, 102-107, 111, 115, 126, 166, 171, 175, 180
/opt/odoo_modules/sample_module/radicale/__init__.py                         3      0   100%
/opt/odoo_modules/sample_module/radicale/auth.py                            12      7    42%   8-9, 14-18
/opt/odoo_modules/sample_module/radicale/collection.py                      86     36    58%   14-17, 24, 31, 34, 38, 57-62, 65-66, 68-69, 75, 83, 101, 107-122
/opt/odoo_modules/sample_module/radicale/rights.py                          19     12    37%   10-11, 16-31
/opt/odoo_modules/sample_module/tests/__init__.py                            1      0   100%
/opt/odoo_modules/sample_module/tests/test_sample_module.py                      68     43    37%   28-52, 55, 60-68, 71-74, 77-86, 89-92, 95-101, 104-110, 113-119
/opt/odoo_modules/sample_module/tests/test_collection.py                    56      0   100%
-------------------------------------------------------------------------------------------------
TOTAL                                                                 517    173    67%
```

### Generate a HTML coverage report

1.  make directory
```bash
mkdir /home/odoo/coverage
```

2. add `--html=` parameter to `pytest` command.
```bash
pytest -s --odoo-config=/etc/odoo/odoo.conf --odoo-database=odoo -p no:warnings --html=/home/odoo/coverage/report.html /opt/odoo_modules
```
