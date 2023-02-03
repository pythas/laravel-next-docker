SHELL := bash
.SHELLFLAGS := -euo pipefail -c
MAKEFLAGS += --warn-undefined-variables
MAKEFLAGS += --no-builtin-rules

-include .make/.env
-include .docker/.env

DEFAULT_GOAL := help

# Enable BuildKit
COMPOSE_DOCKER_CLI_BUILD?=1
DOCKER_BUILDKIT?=1

export COMPOSE_DOCKER_CLI_BUILD
export DOCKER_BUILDKIT

# Services
DOCKER_SERVICE_BACKEND:=backend
DOCKER_SERVICE_PHP_BASE:=php-base

# Docker
DOCKER_DIR:=./.docker
DOCKER_COMPOSE_FILE=$(DOCKER_DIR)/docker-compose.yaml
DOCKER_COMPOSE_FILE_DEV=$(DOCKER_DIR)/docker-compose.dev.yaml
DOCKER_COMPOSE_COMMAND:=docker compose
DOCKER_COMPOSE:=ENV=$(ENV) \
	TAG=$(TAG) \
	DOCKER_REGISTRY=$(DOCKER_REGISTRY) \
	DOCKER_NAMESPACE=$(DOCKER_NAMESPACE) \
	$(DOCKER_COMPOSE_COMMAND) -f $(DOCKER_COMPOSE_FILE) -f $(DOCKER_COMPOSE_FILE_DEV)

help:
	@printf "Usage:\n  make `tput setaf 4`<target>`tput sgr0`\n"

.PHONY: init
init:
	@cp .make/.env.example .make/.env
	@cp .docker/.env.example .docker/.env

	@printf "Created `tput setaf 4`.make/.env`tput sgr0`\n"
	@printf "Created `tput setaf 4`.docker/.env`tput sgr0`\n"
	@printf "\n"
	@printf "Update the .env files with your settings before running any other targets.\n"

.PHONY: validate
validate:
	@$(if $(TAG),,$(error TAG is undefined))
	@$(if $(ENV),,$(error TAG is undefined))
	@$(if $(DOCKER_REGISTRY),,$(error DOCKER_REGISTRY is undefined. Run make init first.))
	@$(if $(DOCKER_NAMESPACE),,$(error DOCKER_NAMESPACE is undefined. Run make init first.))

.PHONY: install
install: docker-build
# Install Laravel
	$(DOCKER_COMPOSE) run --rm --no-deps $(DOCKER_SERVICE_BACKEND) \
		composer create-project --prefer-dist laravel/laravel .

# Fix permissions
	sudo chown -R $(shell id -u):$(shell id -g) ./src

# Update Laravel .env
	sed -i "s/DB_HOST=127.0.0.1/DB_HOST=mysql/g" ./src/.env
	sed -i "s/DB_DATABASE=laravel/DB_DATABASE=$(DB_DATABASE)/g" ./src/.env
	sed -i "s/DB_USERNAME=root/DB_USERNAME=$(DB_USERNAME)/g" ./src/.env
	sed -i "s/DB_PASSWORD=/DB_PASSWORD=$(DB_PASSWORD)/g" ./src/.env
	sed -i "s/REDIS_HOST=127.0.0.1/REDIS_HOST=redis/g" ./src/.env

# Install Octane
	$(DOCKER_COMPOSE) run --rm --no-deps $(DOCKER_SERVICE_BACKEND) \
		composer require laravel/octane

# Configure Octane
	$(DOCKER_COMPOSE) run --rm --no-deps $(DOCKER_SERVICE_BACKEND) \
	    php artisan octane:install --server=swoole

.PHONY: docker-build-php
docker-build-php: validate
	docker build --file $(DOCKER_DIR)/images/php-base/Dockerfile --tag $(DOCKER_REGISTRY)/$(DOCKER_NAMESPACE)/$(DOCKER_SERVICE_PHP_BASE)-$(ENV):$(TAG) .

.PHONY: docker-build-image
docker-build-image: validate
	$(DOCKER_COMPOSE) build

.PHONY: docker-build
docker-build: docker-build-php docker-build-image
# Create docker network if it doesn't exist 
	docker network inspect proxy >/dev/null 2>&1 || \
    	docker network create proxy

# Remove orphaned base container
	docker image rm $(DOCKER_REGISTRY)/$(DOCKER_NAMESPACE)/$(DOCKER_SERVICE_PHP_BASE)-$(ENV):$(TAG)

.PHONY: docker-up
docker-up:
	$(DOCKER_COMPOSE) up -d

.PHONY: docker-down
docker-down: 
	$(DOCKER_COMPOSE) down