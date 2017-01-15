FROM php:7.0.6-fpm

# Install nginx
RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62 \
    && echo "deb http://nginx.org/packages/mainline/debian/ jessie nginx" >> /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y nginx --no-install-recommends

# install the PHP extensions we need
RUN apt-get update \
    && apt-get install -y libpng12-dev \
         libjpeg-dev \
         libpq-dev \
         libmcrypt-dev \
         libldap2-dev \
         libldb-dev \
         libicu-dev \
         libgmp-dev \
    && ln -s /usr/lib/x86_64-linux-gnu/libldap.so /usr/lib/libldap.so \
    && ln -s /usr/lib/x86_64-linux-gnu/liblber.so /usr/lib/liblber.so \
    && ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
    && docker-php-ext-install gd \
         mysqli \
         opcache \
         pdo \
         pdo_mysql \
         pdo_pgsql \
         pgsql \
         ldap \
         intl \
         mcrypt \
         gmp \
         zip \
         bcmath \
         mbstring \
         pcntl

# Install Supervisor
RUN apt-get update \
    && apt-get install -y supervisor \
    && mkdir -p /var/log/supervisor

# Add configuration
COPY default.conf /etc/nginx/conf.d/default.conf
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN rm -rf /var/www/html \
    && rm -rf /var/log/nginx \
    && ln -s /home/site/wwwroot /var/www/html \
    && ln -s /home/LogFiles /var/log/nginx

EXPOSE 80

CMD ["/usr/bin/supervisord"]