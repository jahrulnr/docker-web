FROM php:7.2-fpm

RUN apt-get update && \
	apt-get install -y --no-install-recommends \
	git \
	libmemcached-dev \
	libz-dev \
	libpq-dev \
	libssl-dev libssl-doc libsasl2-dev \
	libmcrypt-dev \
	libxml2-dev \
	zlib1g-dev libicu-dev g++ \
	libldap2-dev libbz2-dev \
	curl libcurl4-openssl-dev \
	libenchant-dev libgmp-dev firebird-dev libib-util \
	re2c libpng++-dev \
	libwebp-dev libjpeg-dev libjpeg62-turbo-dev libpng-dev libxpm-dev libvpx-dev libfreetype6-dev \
	libmagick++-dev \
	libmagickwand-dev \
	zlib1g-dev libgd-dev \
	libtidy-dev libxslt1-dev libmagic-dev libexif-dev file \
	sqlite3 libsqlite3-dev libxslt-dev \
	libmhash2 libmhash-dev libc-client-dev libkrb5-dev libssh2-1-dev \
	unzip libpcre3 libpcre3-dev \
	poppler-utils ghostscript libmagickwand-6.q16-dev libsnmp-dev libedit-dev libreadline6-dev libsodium-dev \
	freetds-bin freetds-dev freetds-common libct4 libsybdb5 tdsodbc libreadline-dev librecode-dev libpspell-dev

# fix for docker-php-ext-install pdo_dblib
# https://stackoverflow.com/questions/43617752/docker-php-and-freetds-cannot-find-freetds-in-know-installation-directories
RUN ln -s /usr/lib/x86_64-linux-gnu/libsybdb.so /usr/lib/

RUN docker-php-ext-configure hash --with-mhash && \
	docker-php-ext-install hash
RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl && \
	docker-php-ext-install imap iconv

RUN docker-php-ext-install bcmath bz2 calendar ctype curl dba dom enchant
RUN docker-php-ext-install fileinfo exif ftp gd gettext gmp
RUN docker-php-ext-install interbase intl json ldap mbstring mysqli
RUN docker-php-ext-install opcache pcntl pspell
RUN docker-php-ext-install pdo pdo_dblib pdo_mysql pdo_pgsql pdo_sqlite pgsql phar posix
RUN docker-php-ext-install readline recode
RUN docker-php-ext-install session shmop simplexml soap sockets sodium
RUN docker-php-ext-install sysvmsg sysvsem sysvshm
# RUN docker-php-ext-install snmp

# fix for docker-php-ext-install xmlreader
# https://github.com/docker-library/php/issues/373
RUN export CFLAGS="-I/usr/src/php" && docker-php-ext-install xmlreader xmlwriter xml xmlrpc xsl

RUN docker-php-ext-install tidy tokenizer wddx zend_test zip

# already build in... what they say...
# RUN docker-php-ext-install filter reflection spl standard
# RUN docker-php-ext-install pdo_firebird pdo_oci

# install pecl extension
RUN pecl install imagick && \
	pecl install igbinary && \
	pecl install ssh2-1.0 && \
	pecl install redis-4.0.1 && \
	pecl install memcached-3.0.4 && \
	docker-php-ext-enable imagick igbinary ssh2 redis memcached

# install pecl extension
# RUN pecl install mongodb && docker-php-ext-enable mongodb

RUN yes "" | pecl install msgpack && \
	docker-php-ext-enable msgpack

# install the php memcache extension
RUN set -x \
	&& cd /tmp \
	&& curl -sSL -o php7.zip https://github.com/websupport-sk/pecl-memcache/archive/php7.zip \
	&& unzip php7 \
	&& cd pecl-memcache-php7 \
	&& /usr/local/bin/phpize \
	&& ./configure --with-php-config=/usr/local/bin/php-config \
	&& make \
	&& make install \
	&& echo "extension=memcache.so" > /usr/local/etc/php/conf.d/docker-php-ext-memcache.ini \
	&& rm -rf /tmp/pecl-memcache-php7 php7.zip

# install APCu
RUN pecl install apcu-5.1.8 && \
	pecl install apcu_bc-1.0.3 && \
	docker-php-ext-enable apcu --ini-name docker-php-ext-10-apcu.ini && \
	docker-php-ext-enable apc  --ini-name docker-php-ext-20-apc.ini

# oracle database
# RUN curl -o /tmp/instantclient-sdk.zip   -L https://github.com/bumpx/oracle-instantclient/raw/master/instantclient-sdk-linux.x64-12.1.0.2.0.zip && \
#     curl -o /tmp/instantclient-basic.zip -L https://github.com/bumpx/oracle-instantclient/raw/master/instantclient-basic-linux.x64-12.1.0.2.0.zip
# RUN unzip /tmp/instantclient-basic.zip -d /usr/local/ && \
#     unzip /tmp/instantclient-sdk.zip -d /usr/local/ && \
#     ln -s /usr/local/instantclient_12_1 /usr/local/instantclient && \
#     ln -s /usr/local/instantclient/libclntsh.so.12.1 /usr/local/instantclient/libclntsh.so && \
#     docker-php-ext-configure oci8 --with-oci8=instantclient,/usr/local/instantclient && \
#     docker-php-ext-install oci8

RUN apt-get update -y && apt-get install -y apt-transport-https locales gnupg

# install MSSQL support and ODBC driver
# RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
# 	curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list && \
# 	export DEBIAN_FRONTEND=noninteractive && apt-get update -y && \
# 	ACCEPT_EULA=Y apt-get install -y msodbcsql unixodbc-dev
# RUN set -xe \
# 	&& pecl install pdo_sqlsrv \
# 	&& docker-php-ext-enable pdo_sqlsrv \
# 	&& apt-get purge -y unixodbc-dev && apt-get autoremove -y && apt-get clean

# install GD
RUN docker-php-ext-configure gd \
	--with-gd \
	--with-webp-dir \
	--with-jpeg-dir \
	--with-png-dir \
	--with-zlib-dir \
	--with-xpm-dir \
	--with-freetype-dir && \
	docker-php-ext-install gd

# set locale to utf-8
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen
ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

#RUN pecl install stackdriver_debugger-alpha \
#	&& echo "extension=stackdriver_debugger.so" > /usr/local/etc/php/conf.d/docker-php-ext-stackdriver_debugger.ini

# install xdebug
# RUN pecl install xdebug && docker-php-ext-enable xdebug

#--------------------------------------------------------------------------
# Final Touches
#--------------------------------------------------------------------------

# install required libs for health check
RUN apt-get -y install libfcgi0ldbl nano htop iotop lsof cron mariadb-client redis-tools

# install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
	&& php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
	&& php composer-setup.php \
	&& php -r "unlink('composer-setup.php');" \
	&& mv composer.phar /usr/local/sbin/composer \
	&& chmod +x /usr/local/sbin/composer

# install NewRelic agent
RUN echo 'deb http://apt.newrelic.com/debian/ newrelic non-free' | tee /etc/apt/sources.list.d/newrelic.list && \
	curl https://download.newrelic.com/548C16BF.gpg | apt-key add - && \
	apt-get -y update && \
	DEBIAN_FRONTEND=noninteractive apt-get -y install newrelic-php5 newrelic-sysmond && \
	export NR_INSTALL_SILENT=1 && newrelic-install install

# install SendGrid
RUN echo "postfix postfix/mailname string localhost" | debconf-set-selections && \
	echo "postfix postfix/main_mailer_type string 'Internet Site'" | debconf-set-selections && \
	DEBIAN_FRONTEND=noninteractive apt-get install postfix libsasl2-modules -y

# Set default work directory
# ADD scripts/* /usr/local/bin/
RUN chmod +x  /usr/local/bin/*

# Health check
RUN echo '#!/bin/bash' > /healthcheck && \
	echo 'SCRIPT_NAME=/health SCRIPT_FILENAME=/health REQUEST_METHOD=GET cgi-fcgi -bind -connect 127.0.0.1:9000 || exit 1' >> /healthcheck && \
	chmod +x /healthcheck

# Clean up
RUN apt-get remove -y git && apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
