FROM ubuntu:focal

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get dist-upgrade -y

RUN apt-get install -y net-tools bind9-utils telnet

RUN apt-get install postgresql -y

RUN rm -rf /var/cache/apt/* && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/log/apt/*

USER postgres

RUN /etc/init.d/postgresql start && \
     psql -c 'CREATE ROLE "rails01";' && \
     psql -c 'CREATE DATABASE "rails01";' && \
     psql -c 'ALTER DATABASE "rails01" OWNER TO "rails01";' && \
     psql -c 'GRANT ALL PRIVILEGES ON DATABASE "rails01" TO "rails01";' && \
     psql -c 'ALTER ROLE "rails01" LOGIN;' && \
     psql -c "ALTER ROLE \"rails01\" PASSWORD 'rails01';" && \
     psql -c 'ALTER DATABASE "rails01" SET search_path="rails01";' && \
     psql -d 'rails01' -c 'DROP SCHEMA "public";' && \
     psql -d 'rails01' -c 'CREATE SCHEMA "rails01"'\ && \
     psql -d 'rails01' -c 'ALTER SCHEMA "rails01" OWNER TO "rails01";' && \
     psql -d 'rails01' -c 'GRANT ALL PRIVILEGES ON SCHEMA "rails01" TO "rails01";'

RUN sed -i "s/#listen_addresses = 'localhost'/listen_addresses = 'postgresql01'/g" /etc/postgresql/12/main/postgresql.conf

COPY ./files/pg_hba.conf /etc/postgresql/12/main/pg_hba.conf

CMD ["/usr/lib/postgresql/12/bin/postgres", "-D", "/var/lib/postgresql/12/main", "-c", "config_file=/etc/postgresql/12/main/postgresql.conf"]