FROM ruby:2.5.1

ENV LANG C.UTF-8

ENV GEM_PACKAGES libxml2-dev libxslt-dev imagemagick

RUN apt-get update -qq && \
    apt-get install -y \
    $GEM_PACKAGES \
    apt-transport-https

# install nodejs
RUN curl -s -L git.io/nodebrew | perl - setup
ENV PATH /root/.nodebrew/current/bin:$PATH
RUN nodebrew install-binary v8.7.0
RUN nodebrew use v8.7.0

# install yarn
# https://yarnpkg.com/en/docs/install#linux-tab
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && \
    apt-get install -y --no-install-recommends apt-transport-https yarn && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir /app
WORKDIR /app

ADD Gemfile /app/Gemfile
ADD Gemfile.lock /app/Gemfile.lock

RUN bundle install

ADD . /app

RUN mkdir -p tmp/sockets
