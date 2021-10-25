We use BlackBox to monitor our applications.
[Blackbox]() is a exporter of Prometheus ecosystem to check if a application is up and the times to response the request.

When we have an issue with this monitoring piece, we need to debug the probe done and analyze the jobs.
You can find the jobs in the BlackBox UI separated by probe, but this UI is only exposed in localhost and we can access from our computer.
We need to create a SSH tunnel to map the remote localhost port with a port in our local machine:

```
ssh -L 9115:monitor.coopdevs.org:9115 monitor.coopdevs.org
```

Once the tunnel is created you can access to the BlackBox UI in `localhost:9115`.

