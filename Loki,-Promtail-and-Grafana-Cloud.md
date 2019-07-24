Hot links:
* [Grafana cloud account dashboard](https://grafana.com/orgs/coopdevs/): Manage API keys, members, payment, enable/disable data sources, etc.
* [Grafana instance](https://coopdevs.grafana.net/): Actual grafana site.
  * [Explore](https://coopdevs.grafana.net/explore) (query) data sources directly. Loads automatically labels.
  * Use and manage [Dashboards](https://coopdevs.grafana.net/dashboards): prepared queries shown in panels - tiled graphs.
* [[Grafana cloud credentials]] onboarding process

---

We hired Grafana Cloud services. This provides us:
* Visualisation:  [Grafana instance](https://coopdevs.grafana.net/)
* Metrics: [Prometheus data storage](https://grafana.com/products/cloud#prometheus-details)
* Logs: [Loki server](https://grafana.com/oss/loki#about)
* [Customer support](https://grafana.com/contact)

This doesn't provide us:
* Metrics: [Prometheus server](https://prometheus.io/docs/introduction/overview/)  
  See [Ansible playbook to provision a Prometheus instance](https://gitlab.com/coopdevs/monitor-provisioning/)  
  middle-person role: remote-read _from many hosts_ → remote-write _to single prometheus db_
* Logs: [Promtail](https://github.com/grafana/loki/blob/master/docs/promtail.md), logs collector agent
* Mixed logs/metrics dashboards: currently [Loki can only be visualized through Grafana explore](https://grafana.com/docs/features/datasources/loki/#querying-logs).  
  Current progress on this:
  * [Issue in Github: epic](https://github.com/grafana/grafana/issues/14860)
  * [Issue in Github: task](https://github.com/grafana/grafana/issues/14576)
  * [Milestone for this feature](https://github.com/grafana/grafana/milestone/134) (it's being pushed: 6.1, 6.2, 6.3, 6.3.0-beta1, 6.4)
  * [Release publication frequency](https://github.com/grafana/grafana/releases)

---

More resources:
* [How to configure, install and run promtail for Odoo (untested)](https://gitlab.com/snippets/1868202)
* [How to process log lines with promtail before sending them out](https://github.com/grafana/loki/blob/master/docs/logentry/processing-log-lines.md)
* [Grafana docs](https://grafana.com/docs/): installation, usage, concepts...
* [Syntax for querying loki](https://github.com/grafana/loki/blob/master/docs/usage.md)
* [Syntax for querying prometheus](https://prometheus.io/docs/prometheus/latest/querying/basics/)
* API Keys policy: [[Grafana cloud credentials]]
