FROM ubuntu:20.04

ENV VERSION 9.5.5

ENV GLPI_LANG pt_BR

ENV PLUGINS all

ENV DEBIAN_FRONTEND=noninteractive

WORKDIR /var/www/html

RUN apt update && \
apt -qy install \
bzip2 \
apache2 \
php \
php-common \
php-dev \
libmcrypt-dev \
gcc \
make \
autoconf \
libc-dev \
pkg-config \
php-cli \
php-gd \
php-mysqlnd \
php-json \
php-mbstring \
php-mysqli \
php-gd \
php-curl \
php-imap \
php-ldap \
php-opcache \
php-apcu \
php-xmlrpc \
php-intl \
php-zip \
php-pear \
php-cas \
php-bz2 \
cron \
netcat \
&& rm -rf /var/lib/apt/lists/* \
&& pecl channel-update pecl.php.net \
&& pecl update-channels \
&& pecl install mcrypt

COPY php.d /etc/php/7.4/apache2/conf.d/

COPY conf.d/glpi.conf /etc/apache2/sites-available/glpi.conf

COPY crontab /etc/cron.d/glpi
RUN chmod 0644 /etc/cron.d/glpi \
&& crontab /etc/cron.d/glpi

ADD https://github.com/glpi-project/glpi/releases/download/$VERSION/glpi-$VERSION.tgz .
RUN tar -xf glpi-$VERSION.tgz -C /tmp/ \
&& mv /tmp/glpi/* /var/www/html/ \
&& chown -R www-data:www-data /var/www/html \
&& chmod -R 755 /var/www/html \
&& rm -rf /var/www/html/glpi-$VERSION.tgz \
&& rm -f /var/www/html/index.html

COPY scripts/glpi-entrypoint.sh /

RUN chmod 755 /glpi-entrypoint.sh \
&& rm -rf /var/www/html/install/install.php \
&& a2enmod rewrite

ADD https://raw.githubusercontent.com/eficode/wait-for/v2.1.2/wait-for .
RUN chmod +x wait-for

COPY pics/ /var/www/html/pics/

EXPOSE 80/tcp

CMD ["/glpi-entrypoint.sh"]
