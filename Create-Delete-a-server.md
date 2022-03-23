These are the MUST BE DONE steps to create/delete a server.

# Create a server
1. Talk with the commission coordinator to update the contract if exists.
2. Create a bucket for the backups in B2.
    1. Create the bucket in B2
    2. Save the credentials in BW with the following template name: `backblaze - <domain>`
3. Create the server in Hetzner.
    1. Use the domain as server name
    2. Add the SSH keys of SysAdmins (your key if you can run the sys_admin role) to give root access.
4. Add a DNS record pointing to the server IP.
5. Configure the provisioning project:
    1. Configure SysAdmin users.
    1. Configure app variables.
    1. Configure backups.
    1. Configure certbot (Let's Encrypt certificates).
    1. Configure monitoring NodeExporter and Postgres Exporter (in case of use PostgreSQL as DB):
        1. Create the BasicAuth credentials
        2. Save it in BW with the template name: `<exporter> - <domain>` Ex: `NodeExporter - odoo.coopdevs.org`
6. Configure Prometheus to scrape this new server and add the target to Blackbox job.
7. Create a Grafana dashboard with alerts.

# Delete a server
1. Talk with the commission coordinator to update the contract if exists.
2. Remove the alerts and dashboards.
3. Remove the Prometheus job configuration related with this server.
4. Archive the repository in Gitlab or publish an MR deleting the host configuration in the provisioning repository.
5. Revoke the certificates:
```
$ sudo certbot certificates  # To check the certificates
...
$ sudo certbot revoke --cert-path /etc/letsencrypt/live/<domain-name>/cert.pem --key-path /etc/letsencrypt/live/<domain-name>/privkey.pem --reason cessationOfOperation  # Revocation reasons: https://en.wikipedia.org/wiki/Certificate_revocation_list#Reasons_for_revocation
```
6. Delete the server and the volumes attached to it in Hetzner.
7. Remove the DNS record pointing to the server IP.
8. Create activity to remove the backups.