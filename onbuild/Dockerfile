FROM fedora:27 AS aws-jars-provider
RUN curl -fLO https://jar-download.com/cache_jars/org.apache.hadoop/hadoop-aws/2.7.3/jar_files.zip \
    && yum install -y unzip && yum clean all \
    && unzip jar_files.zip \
    && rm jar_files.zip

FROM fedora:27 AS hdfs-client-onbuild-base
RUN yum install -y \
    hadoop-hdfs \
    && yum clean all

FROM hdfs-client-onbuild-base AS hdfs-client-krb5-onbuild-base
RUN yum install -y \
    krb5-workstation \
    krb5-libs \
    krb5-auth-dialog \
    && yum clean all

FROM hdfs-client-onbuild-base AS hdfs-client-aws-onbuild-base
COPY --from=aws-jars-provider *.jar /usr/share/hadoop/common/lib/

FROM hdfs-client-krb5-onbuild-base AS hdfs-client-krb5-aws-onbuild-base
COPY --from=aws-jars-provider *.jar /usr/share/hadoop/common/lib/

FROM hdfs-client-onbuild-base AS hdfs-client-onbuild
ONBUILD COPY ./conf/hadoop/*.xml /etc/hadoop/

FROM hdfs-client-krb5-onbuild-base AS hdfs-client-krb5-onbuild
ONBUILD COPY ./conf/krb5/krb5.conf /etc/krb5.conf

FROM hdfs-client-aws-onbuild-base AS hdfs-client-aws-onbuild
ONBUILD COPY ./conf/hadoop/*.xml /etc/hadoop/

FROM hdfs-client-krb5-aws-onbuild-base AS hdfs-client-krb5-aws-onbuild
ONBUILD COPY ./conf/hadoop/*.xml /etc/hadoop/
ONBUILD COPY ./conf/krb5/krb5.conf /etc/krb5.conf
