FROM php:7.0-apache

COPY example /var/www/htdocs/example

RUN apt-get update && apt-get install -y git unzip curl gnupg

##########################################
####       NODE PACKAGES       ##########
##########################################
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash - \
        && apt-get install -y nodejs \
        && npm install -g bower \
        && npm install -g grunt \
        && npm install -g yarn


WORKDIR /var/www/htdocs/example

RUN curl -sS https://getcomposer.org/installer | php \
        && mv composer.phar /usr/local/bin/
        # \
        # && ln -s /usr/local/bin/composer.phar /usr/local/bin/composer

RUN sed -i -e 's@/var/www/html@/var/www/htdocs/example/public@' /etc/apache2/apache2.conf
RUN sed -i -e 's@/var/www/html@/var/www/htdocs/example/public@' /etc/apache2/sites-available/000-default.conf
RUN a2enmod rewrite
RUN usermod -u 1000 www-data

WORKDIR /var/www/htdocs

# add credentials php
COPY bin/docker/conf/credentials.php /var/www/htdocs/example/config/credentials.php
COPY bin/docker/conf/credentials_public.json /var/www/htdocs/example/public/credentials_public.json
COPY dist /var/www/htdocs/example/public/lib/hipay-fullservice-sdk

# run entrypoint
COPY bin/docker /tmp
RUN chmod u+x /tmp/images/sdk-js/entrypoint.sh
ENTRYPOINT ["/tmp/images/sdk-js/entrypoint.sh"]