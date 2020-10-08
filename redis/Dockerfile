FROM ubuntu:focal

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get dist-upgrade -y

RUN apt-get install -y net-tools bind9-utils telnet

RUN apt-get install -y gcc make pkg-config

RUN rm -rf /var/cache/apt/* && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/log/apt/*

RUN useradd -m -d /app -s /bin/bash  redis && \
    chown -R redis:redis /app

WORKDIR /app

ENV REDIS_VERSION=6.0.6

COPY ./archives/redis-${REDIS_VERSION}.tar.gz .

RUN mkdir /app/redis && \
    mkdir /app/redis/${REDIS_VERSION} && \
    mkdir /app/redis/${REDIS_VERSION}/bin && \
    mkdir /app/redis/${REDIS_VERSION}/etc && \
    ln -s /app/redis/${REDIS_VERSION} /app/redis/latest

RUN tar -xvf redis-${REDIS_VERSION}.tar.gz && cd redis-${REDIS_VERSION} && make

RUN cd redis-${REDIS_VERSION} && \
    cp src/redis-server /app/redis/${REDIS_VERSION}/bin && cp src/redis-cli /app/redis/${REDIS_VERSION}/bin && \
    cp src/redis-sentinel /app/redis/${REDIS_VERSION}/bin && cp src/redis-benchmark /app/redis/${REDIS_VERSION}/bin && \
    cp src/redis-check-rdb /app/redis/${REDIS_VERSION}/bin && cp src/redis-check-aof /app/redis/${REDIS_VERSION}/bin && \
    rm -rf redis-${REDIS_VERSION} redis-${REDIS_VERSION}.tar.gz

COPY ./files/redis.conf /app/redis/${REDIS_VERSION}/etc

ENV PATH=/app/redis/latest/bin:$PATH

CMD ["/app/redis/latest/bin/redis-server", "/app/redis/latest/etc/redis.conf"]