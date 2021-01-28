FROM ubuntu:18.04
MAINTAINER thewillonline

# Configure Timezone
ENV TZ 'Europe/Amsterdam'
RUN echo $TZ > /etc/timezone && \
    apt-get update && apt-get install -y tzdata && \
    rm /etc/localtime && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata && \
    apt-get clean

RUN apt-get update -y && \
    apt upgrade && \
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

RUN usermod -a -G www-data nzedb

RUN sed -ri 's/(max_execution_time =) ([0-9]+)/\1 120/' /etc/php/7.2/apache2/php.ini && \
  sed -ri "s/(memory_limit =) (.*$)/\1 -1/" /etc/php/7.2/apache2/php.ini && \
  sed -ri 's/;(date.timezone =)/\1 Europe\/Amsterdam/' /etc/php/7.2/apache2/php.ini && \
  sed -ri 's/(max_execution_time =) ([0-9]+)/\1 120/' /etc/php/7.2/cli/php.ini && \
  sed -ri "s/(memory_limit =) (.*$)/\1 -1/" /etc/php/7.2/cli/php.ini && \
  sed -ri 's/;(date.timezone =)/\1 Europe\/Amsterdam/' /etc/php/7.2/cli/php.ini && \
  sed -ri 's/listen\s*=\s*127.0.0.1:9000/listen = 9000/g' /etc/php/7.2/fpm/pool.d/www.conf && \
  sed -ri 's|;include_path = ".:/php/includes"|include_path = ".:/usr/share/php7.2"|g' /etc/php/7.2/fpm/php.ini && \
  service apache2 restart
  
