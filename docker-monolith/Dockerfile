FROM ubuntu:18.04

RUN apt-get update \
	&& apt-get install -y \
		mongodb-server \
		ruby-full ruby-dev \
		build-essential \
		git \
	&& rm -rf /var/lib/apt/lists/*

RUN gem install bundler
RUN git clone -b monolith https://github.com/express42/reddit.git

COPY mongod.conf /etc/mongod.conf
COPY db_config /reddit/db_config
COPY start.sh /start.sh

RUN cd /reddit && rm Gemfile.lock && bundle install \
	&& chmod 0777 /start.sh

CMD ["/start.sh"]
