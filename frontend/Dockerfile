FROM ubuntu:focal

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get dist-upgrade -y

RUN apt-get install -y net-tools bind9-utils telnet

RUN apt-get install -y apache2

RUN rm -rf /var/cache/apt/* && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/log/apt/*

RUN a2dissite 000-default.conf

COPY ./files/frontend01.conf /etc/apache2/sites-available

RUN a2ensite frontend01.conf
RUN a2enmod proxy
RUN a2enmod proxy_http

RUN sed -i "s/Listen 80//g" /etc/apache2/ports.conf

CMD ["apachectl", "-D", "FOREGROUND"]