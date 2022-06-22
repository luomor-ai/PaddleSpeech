FROM centos:latest
MAINTAINER obpilot
COPY ./obdemo.yaml /tmp/

VOLUME ["/data"]

RUN yum -y install tree net-tools vim sudo passwd \
    && yum -y install mariadb-connector-c \
    && yum install -y yum-utils \
    && yum-config-manager --add-repo https://mirrors.aliyun.com/oceanbase/OceanBase.repo \
    && yum install -y ob-deploy obclient \
    && useradd admin && echo 'admin:adminPWD123' | chpasswd \
    && sed -i '$a\admin    ALL=(ALL)    ALL' /etc/sudoers \
    && mkdir -p /data/1 /data/log1  \
    && chown -R admin.admin /data/ /tmp/ \
    && su admin -c  "obd cluster deploy obdemo -c /tmp/obdemo.yaml"

USER admin
WORKDIR /home/admin
COPY ./bmsql.sql /home/admin/
COPY ./readme.md /home/admin/
