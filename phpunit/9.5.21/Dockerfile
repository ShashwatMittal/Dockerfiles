# PHPUnit Docker Container.
FROM alpine:3.16
LABEL mantainer="Rahul Prajapati <rahul.prajapati@live.in>, Riddhesh Sanghvi <riddhesh237@gmail.com>"

LABEL name="phpunit-9.5.21"

ENV PEAR_PACKAGES foo

WORKDIR /tmp

RUN	echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories && \
    apk update && \
	apk upgrade && \
	apk add --update --no-cache \
        bash \
        ca-certificates \
        curl \
        git \
        subversion \
        libmcrypt-dev \
        php8 \
        php8-bcmath \
        php8-ctype \
        php8-curl \
        php8-dom \
        php8-exif \
        php8-fileinfo \
        php8-iconv \
        php8-json \
        php8-mbstring \
        php8-pecl-mcrypt \
        php8-mysqli \
        php8-opcache \
        php8-openssl \
        php8-pcntl \
        php8-pdo \
        php8-pdo_mysql \
        php8-pdo_pgsql \
        php8-pdo_sqlite \
        php8-phar \
        php8-session \
        php8-simplexml \
        php8-soap \
        php8-tokenizer \
        php8-xdebug \
        php8-xml \
        php8-xmlreader \
        php8-xmlwriter \
        php8-zip \
        php8-zlib \
        unzip \
        mariadb-client \
        php8-pecl-memcached \
        php8-phpdbg \
        sqlite-dev \
        wget \
    && php -r "copy('https://pear.php.net/go-pear.phar', 'go-pear.phar');" \
    && php go-pear.phar \
    && php -r "unlink('go-pear.phar');" \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/bin --filename=composer \
    && php -r "unlink('composer-setup.php');" \
    && composer require "phpunit/phpunit:9.5.21" --prefer-source --no-interaction \
    && composer require "phpunit/php-invoker" --prefer-source --no-interaction \
		&& composer require --dev yoast/phpunit-polyfills \
    && ln -s /tmp/vendor/bin/phpunit /usr/local/bin/phpunit \
    && sed -i 's/nn and/nn, Rahul Prajapati (Docker) and/g' /tmp/vendor/phpunit/phpunit/src/Runner/Version.php \
    # Enable X-Debug
    && sed -i 's/\;z/z/g' /etc/php8/conf.d/50_xdebug.ini \
    && echo 'xdebug.mode=coverage' >> /etc/php8/conf.d/50_xdebug.ini \
    && php -m | grep -i xdebug

ONBUILD RUN \
    { \
        [ "${PEAR_PACKAGES}" != "foo" ]; \
    } || exit 0 && pear install ${PEAR_PACKAGES}

VOLUME ["/app"]

WORKDIR /app

CMD ["/bin/true"]
