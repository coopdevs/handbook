## Intro

In order to feed our data sources for our Grafana, we need to ensure proper authentication. (At least) at Grafana Cloud, each data source can create API keys for different types of clients. Currently, we are using:
* Prometheus data source (Hosted metrics)
* Loki data source (Hosted logs)

## tl;dr

The fastest, dirty way to try something is to use api keys that are shared between clients. We must use these only for testing purposes and not rely on them. We remove and recreate them from time to time in order to let hanging insecure configs around.

You can find those at [Coopdevs bitwarden](https://vault.bitwarden.com) named "Grafana Cloud - API keys".

## Set up a Grafana Cloud instance

1. Done ~~Create a free account at Grafana Cloud → [[https://grafana.com/signup]]~~
2. Done ~~Create an organization and pay for it. Your user will be this organization admin.~~
3. Done ~~Create  a "hosted metrics instance" (prometheus datastore) and a "hosted logs instance" (loki) under the "Grafana Cloud" plan~~

## Give access to a new workmate to Grafana Cloud
4. Create a user for you or another mate.
5. Ask an admin member of this organization to add you to it.
6. Available roles are: Viewer, Editor, Admin

Both [grafana instance](https://coopdevs.grafana.net) and [grafana cloud dahsboard](https://grafana.com/orgs/coopdevs) are accessed with grafana cloud personal accounts.

## Create API keys for a new client app
This will be mostly needed for Promtail. Bear in mind that in case of metrics, we have only one Prometheus server with a single publisher key. If you want to monitor more hosts, see [[Add monitoring to a new host]].

1. Head again to our Grafana Cloud dashboard, to [API Keys section](https://grafana.com/orgs/coopdevs/api-keys).
2. Create your key with MetricsPublisher role:
  * Name wisely: host, client, role
  * Note down the key
3. Switch to the type of instance you want to feed:
  * Prometheus: https://grafana.com/orgs/coopdevs/hosted-metrics/
  * Loki: https://grafana.com/orgs/coopdevs/hosted-logs/
4. Copy configuration and user.
5. Save the credentials (key name + user + key value) at Bitwarden or at the corresponding Ansible Vault.