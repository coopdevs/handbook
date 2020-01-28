This is the systems part of welcoming a new organization to a multi-db odoo. For them to have "their own Odoo", we will add a new database to the Ansible inventory of the multi-db Odoo instance that will host them.

The following sections are adapted from the [Merge Request template to add a new DB at processos.org](https://gitlab.com/coopdevs/odoo-processos-inventory/blob/master/.gitlab/merge_request_templates/modify-dbs.md)


---



### Changes at this repo

For each database change, please do:

1. At host vars file, add or delete the **short name** to/from `odoo_role_odoo_dbs`
2. At host vars file, add or delete the **domain name** to/from `domains`

### Glossary

_Don't modify this one, it's only informative_

| environment | `host_vars` file to modify | domain name | root domain |
| ----------- | -------------------------- | ---------- | ---------- |
| trial       | `inventory/host_vars/proves.processos.org/config.yml`| `example.proves.processos.org` | `proves.processos.org` |
| prod        | `inventory/host_vars/processos.org/config.yml`       | `example.processos.org`        |`processos.org`         |

---

## After the MR is approved

1. [systems] At the DNS provider, add or delete a CNAME record from the **domain name** to the **root domain**
2. [systems] Once the DNS has been propagated, run `odoo-provisioning` with this inventory against trial and/or prod servers.
3. [systems] Import geonames data for your the org's country, following [[How to import postal codes for a country]]
4. [functional] Follow [these steps](https://github.com/coopdevs/handbook/wiki/Passos-configurar-nova-empresa-a-Odoo) in order to create the company/ies.
