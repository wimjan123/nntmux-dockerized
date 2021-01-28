FROM mariadb:10.2
MAINTAINER thewillonline

RUN sed -i -r 's/bind-address.*$/bind-address = 0.0.0.0/' /etc/mysql/my.cnf \
  && sed -i -r 's/port.*$/port = 3306'/ /etc/mysql/my.cnf \
  && sed
