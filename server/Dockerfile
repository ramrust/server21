FROM debian:buster

RUN apt update
RUN apt install -y	wget \
					nginx \
					mariadb-server \
					openssl \
					php-fpm \
					# for sockets
					php-mysql \
					# for maria
					php-mbstring \
					# for many lines
                    php-xml \
					#for parsing between sites
                    php-pdo \
					#data base
                    vim

RUN wget https://wordpress.org/latest.tar.gz && \
	tar zxvf latest.tar.gz && \
	rm latest.tar.gz && \
	mv /wordpress /var/www/html

COPY srcs/wp-config.php /var/www/html/wordpress

RUN wget https://files.phpmyadmin.net/phpMyAdmin/5.1.0/phpMyAdmin-5.1.0-english.tar.gz && \
	tar zxvf phpMyAdmin-5.1.0-english.tar.gz && \
	rm phpMyAdmin-5.1.0-english.tar.gz && \
	mv phpMyAdmin-5.1.0-english /var/www/html/phpmyadmin

COPY ./srcs/config.inc.php ./var/www/html/phpmyadmin

RUN mkdir /etc/ssl/nginx/
RUN openssl req -x509 -nodes -days 30 -subj "/C=RU/ST=Horosho/L=Chto/O=sprosil/CN=rulrikes_first_site" -newkey rsa:2048 -keyout /etc/ssl/nginx/rulrike.key -out /etc/ssl/nginx/rulrike.crt;

COPY srcs/nginx-config /etc/nginx/sites-available/
COPY srcs/*.sh .
RUN rm /etc/nginx/sites-enabled/default && \
	ln -s /etc/nginx/sites-available/nginx-config /etc/nginx/sites-enabled/

RUN chown -R www-data:www-data /var/www
	
RUN service mysql start && \
	mysql --execute="CREATE DATABASE wordpress; \
					CREATE USER 'rulrike'@'localhost'; \
					GRANT ALL PRIVILEGES ON wordpress.* TO 'rulrike'@'localhost'; \
					FLUSH PRIVILEGES;"

EXPOSE 443 80
  
CMD service php7.3-fpm start && \
	service mysql start && \
	service nginx start && \
	bash