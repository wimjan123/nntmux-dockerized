FROM mariadb:10.2
MAINTAINER thewillonline

ADD my.cnf /etc/mysql/my.cnf

ENV TERM dumb

RUN apt-get update \
  && apt-get install -y wget \
  && rm -rf /var/lib/apt/lists/*

#MySQLTuner
ENV MYSQL_TUNER_VERSION 1.7.17
RUN wget -O /mysqltuner.tar.gz https://github.com/major/MySQLTuner-perl/archive/${MYSQL_TUNER_VERSION}.tar.gz \
  && cd / \
  && tar xzvf mysqltuner.tar.gz \
  && mv MySQLTuner-perl-${MYSQL_TUNER_VERSION} mysqltuner \
  && rm /mysqltuner.tar.gz \
  && chmod +x /mysqltuner/mysqltuner.pl

RUN mkdir /entryp/

COPY moduser-entrypoint.sh /entryp/moduser-entrypoint.sh
RUN chmod -R 777 /entryp/
ENTRYPOINT ["/entryp/moduser-entrypoint.sh"]
CMD ["mysqld"]
