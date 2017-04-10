FROM centos:latest
MAINTAINER edward.josette@gmail.com

ENV MARIADB_MAJOR=10.1

# MariaDB Repo
COPY MariaDB.repo /etc/yum.repos.d/MariaDB.repo

# Install required packages and MariaDB Vendor Repo
RUN  yum -y update && yum clean all && yum -y install epel-release && \
      rpm --import https://yum.mariadb.org/RPM-GPG-KEY-MariaDB && \
      groupadd -g 250 -r mysql && useradd -u 250 -r -g mysql mysql && \
      yum -y install MariaDB-server MariaDB-client galera less which socat pwgen && yum clean all && \
      mkdir /docker-entrypoint-initdb.d
    

RUN  rm -rf /var/lib/mysql && mkdir /var/lib/mysql  && chown -R mysql:mysql /var/lib/mysql

RUN  sed 's/SELINUX=.*/SELINUX=disabled/g' /etc/sysconfig/selinux

VOLUME /var/lib/mysql

COPY my.cnf /etc/my.cnf.d/my.cnf
COPY cluster.cnf /etc/mysql/conf.d/cluster.cnf

COPY docker-entrypoint.sh /docker-entrypoint.sh
ENTRYPOINT ["/docker-entrypoint.sh"]

EXPOSE 3306 4444 4567 4568
CMD ["mysqld"]
