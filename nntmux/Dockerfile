FROM ubuntu:18.04
MAINTAINER thewillonline

# Configure Timezone
ENV TZ 'Europe/Amsterdam'
RUN echo $TZ > /etc/timezone && \
    apt-get update -y && apt-get install -y tzdata && \
    rm /etc/localtime && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata && \
    apt-get clean

RUN apt-get update -y && \
    apt upgrade -y && \
    apt install -y \
    nano sed

RUN apt install -y \
    software-properties-common python-software-properties \
    git make

RUN add-apt-repository -y ppa:ondrej/php && \
    apt update -y

RUN apt install -y \
    php-pear php7.4 php7.4-cli php7.4-dev php7.4-common php7.4-curl php7.4-json \
    php7.4-gd php7.4-mysql php7.4-mbstring php7.4-xml php7.4-intl php7.4-fpm php7.4-bcmath php7.4-zip php-imagick

RUN apt install -y \
    apache2 libapache2-mod-php7.4

COPY NNTmux.conf /etc/apache2/sites-available/NNTmux.conf

RUN sed -i -e 's/AllowOverride None/AllowOverride All/g'/etc/apache2/apache2.conf && \
    a2dissite 000-default && \
    a2ensite NNTmux.conf && \
    a2enmod rewrite && \
    service apache2 restart

RUN usermod -a -G www-data nntmux

RUN sed -ri 's/(max_execution_time =) ([0-9]+)/\1 120/' /etc/php/7.4/apache2/php.ini && \
  sed -ri "s/(memory_limit =) (.*$)/\1 -1/" /etc/php/7.4/apache2/php.ini && \
  sed -ri 's/;(date.timezone =)/\1 Europe\/Amsterdam/' /etc/php/7.4/apache2/php.ini && \
  sed -ri 's/(max_execution_time =) ([0-9]+)/\1 120/' /etc/php/7.4/cli/php.ini && \
  sed -ri "s/(memory_limit =) (.*$)/\1 -1/" /etc/php/7.4/cli/php.ini && \
  sed -ri 's/;(date.timezone =)/\1 Europe\/Amsterdam/' /etc/php/7.4/cli/php.ini && \
  sed -ri 's/listen\s*=\s*127.0.0.1:9000/listen = 9000/g' /etc/php/7.4/fpm/pool.d/www.conf && \
  sed -ri 's|;include_path = ".:/php/includes"|include_path = ".:/usr/share/php7.4"|g' /etc/php/7.4/fpm/php.ini && \
  service apache2 restart

RUN apt install -y \
    unrar p7zip-full mediainfo lame ffmpeg libav-tools

RUN newgrp www-data && \
    cd /var/www/ && \
    mkdir -p /var/www && \
    chown www-data:www-data /var/www && \
    chmod 775 /var/www

RUN cd /var/www && \
    chmod 777 . && \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php -r 'if (hash_file("SHA384", "composer-setup.php") === preg_filter("#^.*?<pre>|</pre>.*$#s", "", file_get_contents("https://composer.github.io/pubkeys.html"))) { echo "Installer verified"; } else { echo "Installer corrupt"; unlink("composer-setup.php"); } echo PHP_EOL;' && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
    php -r "unlink('composer-setup.php');"

RUN cd /var/www/ && \
    git clone https://github.com/NNTmux/newznab-tmux.git NNTmux && \
    cd /var/www/NNTmux && \
    git fetch --all --tags --prune && \
    git checkout master

RUN cd /var/www/NNTmux && \
    composer install

RUN cd /var/www/NNTmux && \
    chown -R nntmux:www-data /var/www/NNTmux/bootstrap/cache/ && \
    chown -R nntmux:www-data /var/www/NNTmux/storage/logs/ && \
    chown -R nntmux:www-data /var/www/NNTmux/resources/tmp/ && \
    chown -R nntmux:www-data /var/www/NNTmux/public/ && \
    chmod -R 755 /var/www/NNTmux/vendor/ && \
    chmod -R 777 /var/www/NNTmux/storage/ && \
    chmod -R 777 /var/www/NNTmux/resources/ && \
    chmod -R 777 /var/www/NNTmux/public/ && \
    php artisan nntmux:install
