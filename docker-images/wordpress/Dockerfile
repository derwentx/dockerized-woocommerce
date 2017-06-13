FROM php:5.6-apache
MAINTAINER Pierre Martin<hickscorp@gmail.com>

ENV SCRIPTS_DIR /scripts
VOLUME /var/www/html

COPY bin/ /usr/local/bin/

RUN set -e &&\
  echo "----> Starting..." &&\
  WP_CLI_URL=https://github.com/wp-cli/wp-cli/releases/download/v1.2.1/wp-cli-1.2.1.phar &&\
  DEPENDENCIES="\
    libdbd-mysql-perl libdbi-perl libicu52 libjpeg62-turbo libmcrypt4 libmysqlclient18 \
    libpng12-0 libterm-readkey-perl mailutils mysql-client" &&\
  DEV_DEPENDENCIES="\
    icu-devtools libcurl4-openssl-dev libicu-dev libjpeg-dev libmcrypt-dev \
    libpng12-dev libxml2-dev zlib1g-dev g++" &&\

  echo "----> Installing system packages..." &&\
  apt-get update && apt-get install -y $DEV_DEPENDENCIES $DEPENDENCIES &&\

  echo "----> Installing PHP extensions..." &&\
  docker-php-ext-install curl gd iconv intl mbstring mcrypt mysqli opcache \
                         pdo pdo_mysql soap xml zip &&\

  echo "----> Configuring and adding customizations..." &&\
  a2enmod rewrite &&\
  mkdir -p /scripts/{pre,post}-install.d &&\
  curl -L $WP_CLI_URL -o /usr/local/bin/wp &&\
  chmod +x /usr/local/bin/wp \
           /usr/local/bin/apache2-foreground \
           /usr/local/bin/entrypoint &&\
  echo "sendmail_path=sendmail -i -t" >> /usr/local/etc/php/conf.d/php-sendmail.ini &&\

  echo "----> Cleaning up..." &&\
  apt-get remove --purge -y $DEV_DEPENDENCIES &&\
  apt-get autoremove -y &&\
  apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENTRYPOINT ["/usr/local/bin/entrypoint"]
CMD ["apache2-foreground"]
