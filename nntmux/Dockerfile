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
    apt install -y nano

RUN apt install -y \
    software-properties-common python-software-properties \
    git make

RUN add-apt-repository -y ppa:ondrej/php && \
    apt update -y

RUN apt install -y \
    php-pear php7.4 php7.4-cli php7.4-dev php7.4-common php7.4-curl php7.4-json \
    php7.4-gd php7.4-mysql php7.4-mbstring php7.4-xml php7.4-intl php7.4-fpm php7.4-bcmath php7.4-zip php-imagick
