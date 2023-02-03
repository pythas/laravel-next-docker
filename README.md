# Docker Laravel + Next.js template
Dockerized Laravel with Octane and Swoole PHP Server, MySQL & Next.js template.

## Setup API-backend

Initalize .env-configuration:
```bash
cd api
make init
```

Update `.docker/.env` and `.make/.env` with your configuration.

Build containers and install Laravel and Octane:

```bash
make install
```

Start the containers:

```bash
make docker-up
```

To rebuild the containers:

```bash
make docker-build
```

## Setup frontend

```
TODO.
```