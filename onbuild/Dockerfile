FROM fedora:27

RUN yum install -y \
    hadoop-hdfs \
    krb5-workstation \
    krb5-libs \
    krb5-auth-dialog \
    && yum clean all

ONBUILD COPY ./conf/hadoop/*.xml /etc/hadoop/
ONBUILD COPY ./conf/krb5/krb5.conf /etc/krb5.conf