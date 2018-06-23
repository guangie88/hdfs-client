# `hdfs-client`

Builds on
[`hdfs-client-onbuild`](https://github.com/guangie88/hdfs-client-onbuild).

## Original Variant

This variant uses the same `core-site.xml` and `krb5.conf` files for
`ONBUILD COPY` from the [Onbuild Variant](`onbuild-variant`).

## Xenon Variant

This variant contains the required client settings for
[xenon-hdfs-kerberos](`https://hub.docker.com/r/nlesc/xenon-hdfs-kerberos/`) to
work.

You will need to run `nlesc/xenon-hdfs-kerberos` with additional TCP + UDP port
forwarding for port 88. So the `docker run` command should become:

```bash
docker run --detach --name=xenon-hdfs-kerberos --hostname xenon-hdfs-kerberos \
-p 88:88/tcp -p 88:88/udp -p 8020:8020 -p 50010:50010 -p 50470:50470 \
-p 50475:50475 nlesc/xenon-hdfs-kerberos
```

For this docker client, use `kinit xenon` instead of just `kinit`, and the
password is `javagat`.

You are likely to require the use of `docker run` flag `--net=host`, since the
ports to connect to are only exposed to the host.

For example:

```bash
docker run --net=host -it guangie88/hdfs-client-krb5-xenon:latest bash
```

Assuming that the image `nlesc/xenon-hdfs-kerberos` is running correctly,
within the docker bash:

```bash
kinit xenon
hdfs dfs -df
hdfs dfs -ls /
```
