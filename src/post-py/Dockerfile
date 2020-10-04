FROM python:3.6-alpine as python-base

# 
FROM python-base as builder

RUN mkdir /install
WORKDIR /install

copy requirements.txt /requirements.txt

RUN apk --no-cache --update add build-base && \
    pip install --prefix=/install -r /requirements.txt && \
    apk del build-base

# 
FROM python-base
LABEL version="2.1"

COPY --from=builder /install /usr/local
COPY . /app
WORKDIR /app

ENV POST_DATABASE_HOST post_db
ENV POST_DATABASE posts

ENTRYPOINT ["python3", "post_app.py"]

