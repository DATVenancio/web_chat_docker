#FROM python:3.6.10-alpine3.11
FROM python:3.9.19-alpine3.19

# Sane defaults for pip
ENV PIP_NO_CACHE_DIR off
ENV PIP_DISABLE_PIP_VERSION_CHECK on

RUN apk add --update build-base linux-headers pcre pcre-dev
RUN apk add openssl-dev

COPY requirements.txt /app/
RUN pip install --upgrade pip && \
    pip install -r /app/requirements.txt

RUN addgroup -g 1000 -S uwsgi && \
    adduser -u 1000 -S uwsgi -G uwsgi

COPY ./app /app

WORKDIR /app/

# Logging
RUN mkdir -p /var/log/uwsgi/app/

# Cleanups
RUN apk del build-base linux-headers pcre-dev

EXPOSE 5010
EXPOSE 1000

CMD uwsgi --master \
  --single-interpreter \
  --uid uwsgi \
  --gid uwsgi \
  --workers 1 \
  --gevent 2000 \
  --http :5000 \
  --http-websockets \
  --protocol http \
  --socket 0.0.0.0:1000 \
  --module app:app