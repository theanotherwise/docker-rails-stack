FROM ubuntu:focal

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get dist-upgrade -y

RUN apt-get install -y net-tools bind9-utils telnet

RUN apt-get install -y xz-utils rsync make gcc g++ openssl build-essential tzdata curl git-core && \
    apt-get install -y libqdbm++-dev libyaml-dev libssl-dev libpq-dev zlib1g zlib1g-dev libqdbm-dev libgdbm-dev && \
    apt-get install -y zlib1g-dev libssl-dev libreadline-dev libyaml-dev libsqlite3-dev libmysqlclient-dev

RUN rm -rf /var/cache/apt/* && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/log/apt/*

RUN useradd -m -d /app -s /bin/bash  rails && \
    chown -R rails:rails /app

USER rails

WORKDIR /app

ENV NODE_VERSION=12.18.3
ENV YARN_VERSION=1.22.4
ENV RUBY_VERSION=2.7.1

COPY ./archives/node-v${NODE_VERSION}-linux-x64.tar.xz .
COPY ./archives/ruby-${RUBY_VERSION}.tar.gz .
COPY ./archives/yarn-v${YARN_VERSION}.tar.gz .

RUN mkdir /app/node.js && \
    mkdir /app/node.js/${NODE_VERSION} && \
    ln -s /app/node.js/${NODE_VERSION} /app/node.js/latest

RUN mkdir /app/yarn && \
    mkdir /app/yarn/${YARN_VERSION} && \
    ln -s /app/yarn/${YARN_VERSION} /app/yarn/latest

RUN mkdir /app/ruby && \
    mkdir /app/ruby/${RUBY_VERSION} && \
    ln -s /app/ruby/${RUBY_VERSION} /app/ruby/latest

RUN tar -xvf node-v${NODE_VERSION}-linux-x64.tar.xz && \
    rm -f node-v${NODE_VERSION}-linux-x64.tar.xz && \
    rsync -a node-v${NODE_VERSION}-linux-x64/* /app/node.js/${NODE_VERSION} && \
    rm -rf node-v${NODE_VERSION}-linux-x64

RUN tar -xvf yarn-v${YARN_VERSION}.tar.gz && \
    rm -f yarn-v${YARN_VERSION}.tar.gz && \
    rsync -a yarn-v${YARN_VERSION}/* /app/yarn/${YARN_VERSION} && \
    rm -rf yarn-v${YARN_VERSION}

RUN tar -xvf ruby-${RUBY_VERSION}.tar.gz && \
    rm -f  ruby-${RUBY_VERSION}.tar.gz && \
    cd ruby-${RUBY_VERSION} && ./configure --prefix=/app/ruby/${RUBY_VERSION} && make && make install && cd ../ && \
    rm -rf ruby-${RUBY_VERSION}

ENV PATH=/app/node.js/latest/bin:$PATH
ENV PATH=/app/yarn/latest/bin:$PATH
ENV PATH=/app/ruby/latest/bin:$PATH

RUN gem update --system --no-document
RUN gem update --no-document
RUN gem cleanup

RUN gem install rails --no-document

RUN bundle config --local disable_platform_warnings true && \
    mkdir /app/project

WORKDIR /app/project

RUN rails new . --database=postgresql

COPY ./files/database.yml config/database.yml

RUN pwd && cat config/database.yml | grep -Ev "^[ \t]*#|^$"

RUN rm -f /app/project/tmp/pids/server.pid

COPY ./files/config/environments/development.rb config/environments/development.rb

CMD ["rails", "server", "--binding=rails01", "--port=8080", "--environment=development", "--using=puma"]