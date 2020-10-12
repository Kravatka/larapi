FROM php:7.2-fpm

COPY id_rsa /root/.ssh/id_rsa
COPY id_rsa.pub /root/.ssh/id_rsa.pub

RUN chmod 600 /root/.ssh/id_rsa
RUN chmod 600 /root/.ssh/id_rsa.pub

RUN touch uploads.ini
RUN echo "upload_max_filesize = 100M;" >> /usr/local/etc/php/conf.d/uploads.ini
RUN echo "post_max_size = 100M;" >> /usr/local/etc/php/conf.d/uploads.ini

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install sockets

RUN apt-get update -y \
	&& apt-get install -y \
	sendmail \
	libpng-dev \
	libwebp-dev \
	libjpeg62-turbo-dev \
	libpng-dev \
	libxpm-dev \
	libfreetype6-dev \
	zlib1g-dev \
	libpq-dev \
	nano \
	git \
	libmcrypt-dev \
	curl \
	openssl

#RUN docker-php-ext-configure gd \
#	--with-gd \
#	--with-webp-dir \
#	--with-jpeg-dir \
#    --with-png-dir \
#    --with-zlib-dir \
#    --with-xpm-dir \
#    --with-freetype-dir \
#    --enable-gd-native-ttf

RUN docker-php-ext-install mbstring \
	zip \
	gd \
	pdo \
	pdo_pgsql \
	pdo_mysql \
	calendar

RUN yes | pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_handler=dbgp" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_host=172.10.1.1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_mode=req" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.idekey=PHPSTORM" >> /usr/local/etc/php/conf.d/xdebug.ini

#RUN docker-php-ext-enable xdebug

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apt install -y cron

RUN mkdir /var/www/html/uploads
RUN chown -R www-data:www-data /var/www/html/uploads
