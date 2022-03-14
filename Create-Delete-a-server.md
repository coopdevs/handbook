These are the MUST BE DONE steps to create/delete a server.

# Create a server
1. Talk with the commission coordinator to update the contract if exists.
2. Create a bucket for the backups in B2.
3. Create the server in Hetzner.
4. Add a DNS record pointing to the server IP.
5. Configure the provisioning project:
  * Configure SysAdmin users.
  * Configure app variables.
  * Configure backups.
  * Configure certbot (Let's Encrypt certificates).
  * Configure monitoring: NodeExporter and Postgres Exporter (in case of use PostgreSQL as DB).
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