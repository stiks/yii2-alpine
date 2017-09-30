FROM alpine:edge

MAINTAINER Kirill Garbar <kirill@iterium.co.uk>

RUN echo 'http://dl-cdn.alpinelinux.org/alpine/edge/testing' >> /etc/apk/repositories && \
    apk --update add \
				tzdata \
        php7 \
        php7-ctype \
        php7-mbstring \
        php7-openssl \
        php7-fileinfo \
        php7-dom \
        php7-curl \
        php7-fpm \
        php7-cli \
        php7-pdo \
        php7-mcrypt \
        php7-mysqli \
				php7-opcache \

        # required by composer
        php7-phar \
        php7-json \
        php7-zlib \
        php7-xml \
        php7-tokenizer \
        php7-xmlwriter \

        # this add 30mb to image
        #php7-intl \

        # the rest
        php7-bcmath \
        php7-iconv \
        php7-posix \
        php7-session \
        php7-zip \
		&& cp /usr/share/zoneinfo/Europe/London /etc/localtime \
    && echo "Europe/London" > /etc/timezone \
    && rm -rf /var/cache/apk/*

WORKDIR /app

# Install Composer
ONBUILD ARG GITHUB_OAUTH_TOKEN

# Install composer global bin
ONBUILD RUN php -r "readfile('https://getcomposer.org/installer');" | php -- --install-dir=/usr/local/bin --filename=composer \
    && composer config -g github-oauth.github.com $GITHUB_OAUTH_TOKEN \
    && composer global require hirak/prestissimo \
    && composer global require "fxp/composer-asset-plugin:^1.2.0"

#COPY php.ini /etc/php7/conf.d/50-setting.ini
COPY php-fpm.conf /etc/php7/php-fpm.d/www.conf

EXPOSE 9000

CMD ["php-fpm"]
