## Check the disk usage

```
$ df -h
```

## Find the biggest files in /

```
$ sudo du -a / | sort -n -r | head -n 20
```