FROM ruby:2.7-alpine as ruby-base


LABEL version="2.1"

ENV APP_HOME /app
WORKDIR $APP_HOME

RUN apk add --no-cache --update ruby-dev build-base \
    && gem install bundler 


COPY Gemfile* $APP_HOME/
RUN bundle update --bundler \
    && bundle install \
    && apk del ruby-dev build-base \
    && rm -rf /var/cache/apk/*

COPY . $APP_HOME

ENV POST_SERVICE_HOST post
ENV POST_SERVICE_PORT 5000
ENV COMMENT_SERVICE_HOST comment
ENV COMMENT_SERVICE_PORT 9292

CMD ["puma"]
