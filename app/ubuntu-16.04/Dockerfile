# Build image
FROM monitauro/devbase-supervisor:ubuntu-16.04 

LABEL description="Slimmed-down base image to use for dev with php" \
      maintainer="MoniTauro < twitter @m0n1t4ur0 >" \
      vendor=Monitauro \
      name="monitauro.devbase.php.ubuntu-16.04"

RUN echo "monitauro.devbase.php.ubuntu-16.04" >> /image.txt

RUN cat /image.txt

ENV DEBIAN_FRONTEND=noninteractive \
    NGINX_VERSION=1.16.0 \
    NGINX_USER=www-data \
    NGINX_CONF_PATH=/etc/nginx \
    NGINX_LOG_PATH=/var/log/nginx \
    NGINX_BUILD=Ubuntu \
    PCRE_VERSION=8.43 \
    PCRE_PATH=/var/lib/pcre \
    OPENSSL_VERSION=openssl-1.0.2o

RUN LC_ALL=C.UTF-8 apt-add-repository -y ppa:ondrej/php \
    && apt-get update -y \
    && apt-get install --no-install-recommends -yq \
       autoconf \
       build-essential \
       jq \
       gcc \
       git \
       libjansson-dev \
       libxslt1-dev \
       libc6-dev \
       libexpat-dev \
       libgeoip-dev \
       libpcre3-dev \
       libssh2-1-dev \
       libssl-dev \
       libyaml-dev \
       lsb-release \
       make \
       openssl \
       re2c \
       software-properties-common \
       unzip \
       wget \
       zlib1g-dev

# Install php 7.x with libs and other software
RUN apt-get \
    -o Dpkg::Options::="--force-confdef" \
    -o Dpkg::Options::="--force-confold" \
    install -y -f --no-install-recommends \
        php7.2 \
        php7.2-dev \
        php7.2-fpm \
        php7.2-common \
        php7.2-mysql \
        php7.2-gmp \
        php7.2-curl \
        php7.2-intl \
        php7.2-mbstring \
        php7.2-xmlrpc \
        php7.2-gd \
        php7.2-xml \
        php7.2-cli \
        php7.2-zip

COPY ./configs/etc/php/7.2/ /etc/php/7.2/

# Nginx dependencies
RUN apt-get \
    -o Dpkg::Options::="--force-confdef" \
    -o Dpkg::Options::="--force-confold" \
    install -y -f --no-install-recommends \
        libexpat-dev \
        libmaxminddb-dev \
        mmdb-bin \
        libmaxminddb0

# Turn the detached message off
RUN git config --global advice.detachedHead false

COPY ./installers /installers

# Update openssl from openssl 1.0.2g to 1.0.2o
RUN bash /installers/openssl.sh

# configure NGINX from source
RUN bash /installers/nginx.sh
COPY ./configs/etc/nginx/nginx.conf /etc/nginx/nginx.conf
RUN mkdir -p /usr/share/GeoIP \
    && wget -qO - \
      http://geolite.maxmind.com/download/geoip/database/GeoLite2-City.mmdb.gz \
      | gunzip -c > /usr/share/GeoIP/maxmind-city.mmdb \
    && wget -qO - \
      http://geolite.maxmind.com/download/geoip/database/GeoLite2-Country.mmdb.gz \
      | gunzip -c > /usr/share/GeoIP/maxmind-country.mmdb
COPY ./configs/etc/nginx/ /etc/nginx/

# Install mysql-server
RUN apt-get \
    install -y -f --no-install-recommends \
        mysql-server
ADD start-mysqld.sh /start-mysqld.sh

# Supervisor && logrotate
COPY ./configs/etc/supervisor/ /etc/supervisor/
#COPY ./configs/etc/logrotate.d/ /etc/logrotate.d/

# Create required directories
RUN mkdir -p \
    /app \
    /tmp/xdebug-{profile,outpt} \
    /run/php \
    /var/log/php \
    /var/lib/nginx/{cache,proxy}

VOLUME /app

WORKDIR /app

EXPOSE 9001 80 3306

# Local Variables:
# tab-width: 4
# End: