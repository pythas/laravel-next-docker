ARG BASE_IMAGE
FROM ${BASE_IMAGE} as base

RUN apt-get update \
    && apt-get install -y cron \
    && rm -rf /var/lib/apt/lists/*

COPY ./images/schedule/cron.d/crontab /etc/cron.d/crontab
RUN crontab /etc/cron.d/crontab

WORKDIR /var/www/html

CMD ["cron", "-f"]

FROM base as dev
