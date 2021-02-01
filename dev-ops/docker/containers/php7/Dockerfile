FROM webdevops/php-apache-dev:7.0

ENV COMPOSER_HOME=/.composer
ENV WEB_DOCUMENT_ROOT=/var/www/shopware/shopware

COPY wait-for-it.sh /usr/local/bin/
COPY php-config.ini /usr/local/etc/php/conf.d/
COPY xdebug.ini /usr/local/etc/php/conf.d/
COPY createuser.sh /addExternalUser

# https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=863199
RUN mkdir -p /usr/share/man/man1 \
    && curl -sL https://deb.nodesource.com/setup_12.x | bash
RUN mkdir -p /.npm \
    && apt-get install -y nodejs \
    && npm install -g grunt-cli

RUN docker-php-ext-enable xdebug

RUN apt-get update && apt-get install -y ant default-mysql-client

RUN mkdir /tmp/ioncube && \
    mkdir -p /usr/local/ioncube && \
    cd /tmp/ioncube && \
    curl -SL http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz -o /tmp/ioncube/ioncube_loaders_lin_x86-64.tar.gz && \
    tar xvf /tmp/ioncube/ioncube_loaders_lin_x86-64.tar.gz && \
    cd `php -i | grep extension_dir | cut -d' ' -f 5` && \
    cp /tmp/ioncube/ioncube/ioncube_loader_lin_7.0.so /usr/local/ioncube/ioncube_loader_lin_7.0.so && \
    echo zend_extension=/usr/local/ioncube/ioncube_loader_lin_7.0.so > /usr/local/etc/php/conf.d/00-ioncube.ini && \
    rm -rf /tmp/ioncube/

RUN chmod +x /usr/local/bin/wait-for-it.sh \
&& ln -s /app/psh.phar /bin/psh

WORKDIR /var/www/shopware
