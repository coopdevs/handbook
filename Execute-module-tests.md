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

# Configure CI

We are using the [MQT](https://github.com/odoo-dominicana/maintainer-quality-tools) to run the tests in the Gitlab CI.

In the `sample_files` config you can find an example ob `gitlab-ci.yml` to configure in your custom module:

https://github.com/odoo-dominicana/maintainer-quality-tools/blob/master/sample_files/.gitlab-ci.yml
