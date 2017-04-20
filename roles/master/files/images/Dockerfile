FROM centos:centos7
LABEL MAINTAINER edward.josette@gmail.com

ENV MARIADB_MAJOR=10.1
ENV container=docker

# MariaDB Repo
COPY MariaDB.repo /etc/yum.repos.d/MariaDB.repo

RUN groupadd -g 250 -r mysql && useradd -u 250 -r -g mysql mysql && \
    mkdir -p /var/lib/mysql


RUN  chown -R mysql:mysql /var/lib/mysql

# Install required packages and MariaDB Vendor Repo
RUN  yum -y update && yum clean all && \
     yum -y install --setopt=tsflags=nodocs epel-release && \
     yum -y swap -- remove systemd-container -- install systemd systemd-libs && \
     rpm --import https://yum.mariadb.org/RPM-GPG-KEY-MariaDB && \
     yum -y install bind-utils pwgen psmisc hostname less which socat && \
     yum -y install --setopt=tsflags=nodocs MariaDB-server MariaDB-client galera && \
     yum -y install vim-minimal net-tools && \
     yum -y install openssh-server openssh-clients && yum clean all && \
     mkdir /docker-entrypoint-initdb.d

COPY server.cnf /etc/my.cnf.d/server.cnf

# Add this line for systemd problem

RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

# Without this, init won't start the enabled services and exec'ing and starting
# them reports "Failed to get D-Bus connection: Operation not permitted".
VOLUME /var/lib/mysql
VOLUME /run /tmp
VOLUME [ “/sys/fs/cgroup” ]

RUN systemctl enable mariadb.service
# Till here

COPY docker-entrypoint.sh /docker-entrypoint.sh
RUN chmod +x /docker-entrypoint.sh
ENTRYPOINT ["/docker-entrypoint.sh"]

EXPOSE 3306 4444 4567 4568 22
CMD ["mysqld"]
