FROM ruby:2.4-alpine
MAINTAINER 'Chavez <matthew@el-chavez.me>'

ENV APP_DIR=/usr/src/app
ENV MESSAGES_DIR=/usr/local/sns_message
ENV SNS_PORT=9292

RUN set -x  && \
	apk add --no-cache --virtual \
        .build-deps \
        gcc \
        g++ \
        linux-headers \
        musl-dev \
        make

RUN mkdir -p $APP_DIR
WORKDIR $APP_DIR

COPY Gemfile $APP_DIR/
RUN bundle install

RUN addgroup -S docker && \
    adduser -S -G docker docker && \
    mkdir -p $MESSAGES_DIR && \
    chown docker $MESSAGES_DIR
USER docker

VOLUME $MESSAGES_DIR
EXPOSE $SNS_PORT

CMD fake_sns \
        --bind 0.0.0.0 \
        --database=$MESSAGES_DIR/database.yml \
        --port $SNS_PORT
