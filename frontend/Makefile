SHELL := bash
.SHELLFLAGS := -euo pipefail -c
MAKEFLAGS += --warn-undefined-variables
MAKEFLAGS += --no-builtin-rules

-include .env
-include .docker/.env

DEFAULT_GOAL := help

# Enable BuildKit
COMPOSE_DOCKER_CLI_BUILD?=1
DOCKER_BUILDKIT?=1

export COMPOSE_DOCKER_CLI_BUILD
export DOCKER_BUILDKIT

# Services
DOCKER_SERVICE_FRONTEND:=frontend

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
	@cp .env.example .env
	@cp .docker/.env.example .docker/.env

	@printf "Created `tput setaf 4`.env`tput sgr0`\n"
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
# Install Next.js
	$(DOCKER_COMPOSE) run --rm --no-deps $(DOCKER_SERVICE_FRONTEND) \
		npx --yes create-next-app@latest . --typescript --eslint --use-npm --no-experimental-app --no-src-dir --import-alias "@/*"

# Fix permissions
	sudo chown -R $(shell id -u):$(shell id -g) ./src

.PHONY: docker-build-image
docker-build-image: validate
	$(DOCKER_COMPOSE) build

.PHONY: docker-build
docker-build: docker-build-image

.PHONY: docker-up
docker-up:
	$(DOCKER_COMPOSE) up -d

.PHONY: docker-down
docker-down: 
	$(DOCKER_COMPOSE) down