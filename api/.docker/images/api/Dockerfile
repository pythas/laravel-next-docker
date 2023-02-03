ARG BASE_IMAGE
FROM ${BASE_IMAGE} as base

RUN apt-get update \
# Install Swoole (required by Octane)
    && pecl install swoole \
    && docker-php-ext-enable swoole \
# Install Node (v16 LTS) (required by Octane)
    && curl -sL https://deb.nodesource.com/setup_16.x | bash - \
    && apt-get install -y nodejs \
# Install NPM (required by Octane)
    && npm install -g npm \
# Install Chokidar (required by Octane)
    && npm install --g chokidar \
# Clean up the apt cache
    && rm -rf /var/lib/apt/lists/*

ENV NODE_PATH /usr/lib/node_modules

COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

WORKDIR /var/www/html

EXPOSE 8000

CMD ["php", "artisan", "octane:start", "--server=swoole", "--host=0.0.0.0", "--port=8000", "--max-requests=1000", "--watch"]

HEALTHCHECK --start-period=5s --interval=5s --timeout=5s --retries=3 \
    CMD php artisan octane:status || exit 1

FROM base as dev