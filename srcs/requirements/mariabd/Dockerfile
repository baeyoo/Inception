FROM debian:buster

RUN apt-get update && apt-get upgrade -y

WORKDIR /tmp/mariadb/

COPY src/ /tmp/mariadb/

RUN mkdir -p /run/mysql

# -qq: no output except for errors
RUN apt-get -qq && install mariadb-server -y

# -u: specifies a use name, in this case root
RUN service mysql start && mariadb -u root < /tmp/mariadb/setup.sql

RUN 	sed 's/bind-address            = 127.0.0.1/bind-address            = 0.0.0.0/'\
		-i /etc/mysql/mariadb.conf.d/50-server.cnf

RUN mkdir tempmysql/

RUN cp -r /var/lib/mysql/* tempmysql

EXPOSE 3306

CMD cp -r tempmysql/* /var/lib/mysql/ && mysqld
