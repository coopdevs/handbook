We are using Discourse as a community forum.

You can access to the forum in https://community.coopdevs.org

We install it from https://github.com/discourse/discourse_docker we can find some instructions in the README, but we want to list the most common actions required to manage our instance:

### Access to the server
To access the server we use the user `ubuntu`. This user can use `sudo` commands with a password. You can find the password in the Bitwarden.

```commandline
$ ssh ubuntu@community.coopdevs.org
```

### Finding the project path
And you can find the Discourse repository in:

```commandline
$ cd /var/discourse/
```

> We recommend using the user `root` to run the commands needed... But probably we can change this recommendation...

### Restart the app

To restart the app we have two options:

#### Restart the container with Docker
We can restart directly the Docker container using the `docker` CLI:

```commandline
ubuntu@community:~$ docker ps
CONTAINER ID        IMAGE                 COMMAND             CREATED             STATUS              PORTS                    NAMES
c6ad2d4a8782        local_discourse/app   "/sbin/boot"        8 days ago          Up 27 hours         0.0.0.0:8000->8000/tcp   app
ubuntu@community:~$ docker restart c6ad2d4a878
```

> We don't know if this restart is recommended, but it works and keep the instance healthy. In the case of an unknown app break, it is a good trick.

#### Restart the app with the project `laucher`

https://github.com/discourse/discourse_docker#launcher

> As in our server the repository is deployed with `root` we need `root` access to run this method.
```commandline
ubuntu@community:~# cd /var/discourse/
ubuntu@community:~# ./launcher restart
```

### Check the logs to find some error
As in the previus entry, we can access to the Docker logs or to the `launcher` logs.
### Update the app

TODO