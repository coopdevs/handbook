## Glossary

### Metric name

[metric name](https://prometheus.io/docs/concepts/data_model/#metric-names-and-labels) refers to a measured feature of the system such as *node_cpu_guest_seconds_total*.

### Labels

[Labels](https://prometheus.io/docs/concepts/data_model/#metric-names-and-labels) identify particular instances of a metric name such as *cpu="0",mode="nice"*.

### Time Series

A time series is the list of datapoints (or samples) of the same metric name and set of label name/value pairs. Following the example above *node_cpu_guest_seconds_total{cpu="0",mode="nice"}* identifies time series.

Said expression has a temporal dimension. The node CPU guest second total metric given that CPU and mode will have lots of samples. Prometheus query language then allows to specify a time range as *node_cpu_guest_seconds_total{cpu="0",mode="nice"}[5m]* where the result is the time series of that metric and labels in the last 5 minutes.

```
node_cpu_guest_seconds_total{cpu="0",mode="nice"}
node_cpu_guest_seconds_total{cpu="0",mode="user"}
node_cpu_guest_seconds_total{cpu="1",mode="nice"}
node_cpu_guest_seconds_total{cpu="1",mode="user"}
```

This list has 1 metric and 4 time series.

As a consequence, a datapoint (or sample) has a metric name, a set of label name/value pairs, a timestamp (milliseconds since Unix epoch), and a value (64 bits float number). These are exactly each of the lines Prometheus endpoints expose as text.

```
node_cpu_guest_seconds_total{cpu="0",mode="nice"} 0
node_cpu_guest_seconds_total{cpu="0",mode="user"} 0
node_cpu_guest_seconds_total{cpu="1",mode="nice"} 0
node_cpu_guest_seconds_total{cpu="1",mode="user"} 0
```

This list has 1 metric and 4 datapoints. Note that the last value in the line is the particular value for that metric at that moment in time.

In Prometheus [Query Examples](https://prometheus.io/docs/prometheus/latest/querying/examples/) you'll find an in-depth explanation of all this through its query capabilities.

### Active Series

In the context of Grafana Cloud, an active series is one that you are actively sending datapoints to. When you stop updating a time-series, it is no longer considered "active" for billing purposes.

At each billing cycle, they compute the 95th percentile of the number of active series in the billing period.
