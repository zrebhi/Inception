FROM debian:buster

RUN apt-get -y update && \
	apt-get -y upgrade

# Install php7.3 and its dependencies
RUN apt-get install -y php7.3 \
php-fpm \
php-mysql \
mariadb-client

# Download Wordpress Archive in /var/www, extracts it and deletes the archives, gives root rights to wordpress
RUN apt-get -y install wget
RUN wget https://fr.wordpress.org/wordpress-6.3.1-fr_FR.tar.gz -P /var/www
RUN cd /var/www && tar -xzf wordpress-6.3.1-fr_FR.tar.gz && rm wordpress-6.3.1-fr_FR.tar.gz && \
chown -R root:root /var/www/wordpress

# Change PHP config file
COPY conf/www.conf /etc/php/7.3/fpm/pool.d/www.conf

#Install CLI to configure Wordpress with a script
RUN wget https://github.com/wp-cli/wp-cli-bundle/releases/download/v2.7.0/wp-cli-2.7.0.phar
RUN chmod +x wp-cli-2.7.0.phar
RUN mv wp-cli-2.7.0.phar /usr/local/bin/wp

#Adds the script to our container and executes it
COPY tools/wp_install.sh /var/wp_install.sh
CMD ["bash", "/var/wp_install.sh"]
