Before start with the process read installation instructions on monitor-provision and set it up
## blackbox alert
- edit prometheus.yml in inventory/host_vars/monitor.coopdevs.org
-- add domains on blackbox job targets
- run provision on monitor-provisioning
-- ansible-playbook playbooks/provision.yml --limit=production --ask-vault-pass
- Go to grafana to manually generate the alert
-- TODO