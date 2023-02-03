# Docker Laravel + Next.js template
Dockerized Laravel with Octane and Swoole PHP Server, MySQL & Next.js template. Uses a makefile for some better DX. Only requires docker to be installed on the host.

## Setup backend

Initalize .env configuration:
```bash
cd backend
make init
```

Update `backend/.env` and `backend/.docker/.env` with your configuration.

Build containers and install Laravel and Octane:

```bash
make install
```

Start the containers:

```bash
make docker-up
```

Rebuild the containers:

```bash
make docker-build
```

## Setup frontend

Initalize .env configuration:
```bash
cd frontend
make init
```

Update `frontend/.env` and `frontend/.docker/.env` with your configuration.

Build containers and install Next.js:

```bash
make install
```

Start the containers:

```bash
make docker-up
```

Rebuild the containers:

```bash
make docker-build
```
