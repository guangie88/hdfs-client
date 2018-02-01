# `hdfs-client-krb5`

Use `fedora:27` and include HDFS and Kerberos client related packages only.
Meant only for Kerberos-secured HDFS client connection.

Contain `hdfs` and `kinit`/`klist` commands.

## Onbuild Variant

This should be the variant to use if you are planning to customize both the HDFS
and Kerberos client configuration. `ONBUILD COPY` is performed for both sets of
configuration.

### Hadoop Configuration

Place all the modified XML configurations (e.g. `core-site.xml`) for Hadoop in
`./conf/hadoop/`. Note that the `.` refers to the directory of your own
`Dockerfile`. Unmodified XML files need not appear in the directory, but at
least one XML file must be present due to the limitation of `COPY` command.

### Kerberos Configuration

Place the modified `krb5.conf` for Kerberos in `./conf/krb5/krb5.conf`. Note
that the `.` refers to the directory of your own `Dockerfile`. This file must be
present for the `ONBUILD COPY` to work.

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
