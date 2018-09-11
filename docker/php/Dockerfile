FROM php:fpm-alpine

# Install required php extensions
RUN apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev && \
  docker-php-ext-configure gd \
    --with-gd \
    --with-freetype-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ && \
  NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
  docker-php-ext-install -j${NPROC} gd mysqli opcache && \
  apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev

# opcache configuration
RUN { \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=4000'; \
		echo 'opcache.revalidate_freq=2'; \
		echo 'opcache.fast_shutdown=1'; \
		echo 'opcache.enable_cli=1'; \
	} > /usr/local/etc/php/conf.d/opcache-recommended.ini

# Adds WP-CLI
RUN apk add --no-cache bash && \
    mkdir -p /usr/local/share/wp-cli/ && \
    curl -o /usr/local/share/wp-cli/wp https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \
    chmod +x /usr/local/share/wp-cli/wp && \
    echo '/usr/local/share/wp-cli/wp --allow-root $*' > /usr/local/bin/wp && \
    chmod +x /usr/local/bin/wp

VOLUME /var/www/html
