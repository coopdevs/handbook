## Context

We already learned [[how to add prometheus alerts at Grafana|Adding-alerts-in-Grafana]]. However, we had only tested for Prometheus data sources, and we want them also for logs.

## Limitations of Grafana-Loki 

Currently there's only 1 grafana panel that supports loki datasource, ["Logs"][1], and it looks like we couldn't create a Graph Panel with some statistical processing. However:
*  The explore view shows generates a histogram similar to a histogram dashboard panel
*  LogQL allows some functions such as [counting or averaging][2]
But again, both explore and dashboard return an "Internal Server Error" when we try a query with such a function with Grafana v6.4.4.

## Solution explanation

After mailing grafana.com , they told us how to do it:

> Unfortunately, it is still not possible to use these functions for Loki datasource. This function should be available in Grafana in future releases, when we add the metrics from logs feature. We do not yet have ETA for that though.
> 
> However, there is a sort of a workaround for this, where you can add Loki as a Prometheus datasource. Once that is added, you can use PromQL on logs and get metrics from logs that way.
> 
> Some information about that is available in this video:
https://www.youtube.com/watch?v=J_nAt0XX0qw

## Detailed steps

### Create API key
At [grafana cloud dashboard](https://grafana.com/orgs/coopdevs/api-keys), generate a new key.
- Give it a significant name:  
  _**loki** **viewer** for **prom-loki** datasource_
- Give it only viewer privileges

[[img/api-key]]

### Set up a new data source

1. At our grafana instance, set up a [new datasource](https://coopdevs.grafana.net/datasources/new).
2. Choose **Prometheus** and continue
3. Fill the info copied from your loki data-source but
  * append `/loki` to the url
  * use the recently created api key








[1]: https://grafana.com/docs/features/datasources/loki/#querying-log
[2]: https://github.com/grafana/loki/blob/master/docs/logql.md#counting-logs