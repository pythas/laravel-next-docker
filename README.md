# Docker Laravel + Next.js template
Dockerized Laravel with Octane and Swoole PHP Server, MySQL & Next.js template. Uses a makefile for some better DX.

## Setup backend

Initalize .env configuration:
```bash
cd backend
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

Initalize .env configuration:
```bash
cd frontend
make init
```

Update `.docker/.env` and `.make/.env` with your configuration.

Build containers and install Next.js:

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