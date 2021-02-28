FROM ruby:3.0-alpine3.13

RUN apk add --no-cache libxml2 libxslt build-base libxml2-dev libxslt-dev postgresql-dev tzdata nodejs yarn

RUN gem update --system && \
    gem install nokogiri --platform=ruby -- --use-system-libraries

WORKDIR /app

RUN gem install rails -v 6.1.3

RUN rails new --database=postgresql --no-api --skip-git --skip-test --skip-bundle --skip-webpack-install .

COPY ./files/database.yml config/database.yml

RUN echo -e '\nRails.application.config.hosts = nil' >> config/environments/production.rb
RUN sed -i "s/config.assets.compile = false/config.assets.compile = true/g" config/environments/production.rb

RUN bundle exec rails g controller Home index contact

COPY ./files/routes.rb config/routes.rb

RUN bundle install && \
    bundle exec rails webpacker:install && \
    bundle exec rails webpacker:compile

CMD ["bundle", "exec", "rails", "server", "-b", "0.0.0.0", "-p", "3000"]
