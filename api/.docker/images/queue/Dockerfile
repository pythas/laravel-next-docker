ARG BASE_IMAGE
FROM ${BASE_IMAGE} as base

WORKDIR /var/www/html

CMD ["php", "artisan", "queue:work"]

FROM base as dev