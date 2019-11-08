Grafana alerts are attached to dashboard panels so to get notified on a particular scenario you'll have to have a graph for it first. That can only be done through the UI.

## Alert

Create or choose a graph to base your aler on. Then edit it. This will open the graph's details among which you can configure the alert. You can tweak the evaluation frequency but the default one will surely be fine most of the times. What you need to define always is the threshold above or below which you want to get notified. That is, what you consider a failure that requires us to take action. You can do so from the _Conditions_ section or using the handler from the graph to move the bar up or down.

Note however that queries with variables don't seem to be supported in alerts. You'll have to add a separate query for each variable value instead.

You can see the list of configured alerts and their state at https://coopdevs.grafana.net/alerting/list. There you'll be able to pause/play and edit them without having to reach the graph again.

## Notifications

Notice how this will only be visible from Grafana's dashboards with a red and bliking glow around the affected graphs. To actually notify us of the alert you need to specify a recipient in the _Send to_ field from the _Notifications_ section.

That recipient needs to be defined beforehand as _Channel_ at https://coopdevs.grafana.net/alerting/notifications. Channels can have several types such as Email, webhook, Telegram, Slack, PagerDuty or even Prometheus AlertManager. As of this writing there's a "Sysadmins" defined as email that should suffice for now. Note that channels can include mulitple people, useful for teams.

## Grafana docs

You will find this and many more details around the Alert rule configuration and queries at https://grafana.com/docs/alerting/rules/.

## World Ping

World Ping alerts work the same as described above. The only thing to take into account is that you'll likely need to navigate to the World Ping plugin from the sidebar to reach its dashboards and add alerts on them.
