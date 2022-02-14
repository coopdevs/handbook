The Blackbox Exporter is running in a Docker container inside monitor.coopdevs.org host. It offers a UI interface that include logs, but it can only be accessed from localhost. The way to bypass it is opening a [local port forward](https://linuxize.com/post/how-to-setup-ssh-tunneling/). Here is the command:
```
ssh -L 9115:localhost:9115 monitor.coopdevs.org
```
![imagen](https://user-images.githubusercontent.com/448009/153871341-50d9dd06-c358-4cae-a7d2-46a10db87504.png)

