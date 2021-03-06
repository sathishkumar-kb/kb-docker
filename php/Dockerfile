FROM php:5.6.34-fpm
RUN apt-get update && apt-get install -y libmcrypt-dev wget python

WORKDIR /tmp

ENV LIBRDKAFKA_VERSION=0.9.2

# etiher download from github or PECL rdkafka-beta (latest)
# ENV PHPRDKAFKA_VERSION=0.9.1-php7 

RUN wget https://github.com/edenhill/librdkafka/archive/v${LIBRDKAFKA_VERSION}.tar.gz && \
    tar -xvf v${LIBRDKAFKA_VERSION}.tar.gz && \
    cd librdkafka-${LIBRDKAFKA_VERSION} && \
    ./configure && make && make install && ldconfig && cd /tmp && \
    rm -rf librdkafka-${LIBRDKAFKA_VERSION} v${LIBRDKAFKA_VERSION}.tar.gz

RUN apt-get update

# Install Postgre PDO
RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql

RUN docker-php-ext-install mcrypt mbstring tokenizer mysqli pdo pdo_mysql && \
    pecl install -o redis && \
    pecl install channel://pecl.php.net/rdkafka-beta && \
    rm -rf /tmp/pear && \
    echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini && \
    echo "extension=rdkafka.so" > /usr/local/etc/php/conf.d/rdkafka.ini