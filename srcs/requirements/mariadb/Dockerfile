FROM debian:buster

ARG SQL_DATABASE
ARG SQL_USER
ARG SQL_PASSWORD
ARG SQL_ROOT_PASSWORD

RUN apt update -y && \
    apt upgrade -y && \
    apt install mariadb-server -y && \
    rm -rf /var/lib/apt/lists/*

#Adds our conf file to our container
COPY conf/50-server.cnf	/etc/mysql/mariadb.conf.d/50-server.cnf

#Creates table
#creates user to manipulate table
#gives rights to the user
#changes root user
#refreshes mysql to apply changes
#restarts mySQL to apply changes

RUN service mysql start && \
	mysql -u root -e "CREATE DATABASE IF NOT EXISTS $SQL_DATABASE; \
	CREATE USER IF NOT EXISTS $SQL_USER @'%' IDENTIFIED BY '$SQL_PASSWORD'; \
	GRANT ALL PRIVILEGES ON $SQL_DATABASE.* TO $SQL_USER@'%' IDENTIFIED BY '$SQL_PASSWORD'; \
	ALTER USER 'root'@'localhost' IDENTIFIED BY '$SQL_ROOT_PASSWORD'; \
	FLUSH PRIVILEGES;" && \
	mysqladmin -u root -p$SQL_ROOT_PASSWORD shutdown

CMD ["mysqld_safe"]