FROM ubuntu:xenial

# add our user and group first to make sure their IDs get assigned 
# consistently, regardless of whatever dependencies get added
RUN groupadd -r mysql && useradd -r -g mysql mysql

ENV MYSQL_VERSION 5.6
ENV TERM linux

RUN apt-get update && apt-get install -y openssh-server python-software-properties

RUN apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8


RUN add-apt-repository 'deb http://nyc2.mirrors.digitalocean.com/mariadb/repo/10.1/ubuntu xenial main' && \
    apt-get update && \ 
    apt-get install -y mariadb-server rsync tree net-tools && \
   && rm -rf /var/lib/mysql && mkdir -p /var/lib/mysql && chown -R mysql:mysql /var/lib/mysql 

VOLUME /var/lib/mysql

COPY my.cnf /etc/mysql/my.cnf
COPY cluster.cnf /etc/mysql/conf.d/cluster.cnf

COPY docker-entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

EXPOSE 3306 4444 4567 4568 22
CMD ["mysqld"]
