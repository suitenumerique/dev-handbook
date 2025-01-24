# La Suite numérique with Docker

We extensively use Docker at La Suite numérique. Mostly for developement, but also in production. In this document, you will find a few guidelines on how we write, run and manage our containers.

## Docker/host user mapping

it is commonly assumed that Docker containers **should not** run commands with a privileged account as the `root` user. So it's a good practice to create and declare a `USER` in your `Dockerfile`. When a docker volume is mounted from the host to a container, you may then encounter permission issues with the container's user trying to create new files on the host volume \(_e.g._ when installing dependencies with _npm_\), and this is a good thing! But it is a bit annoying as it may break your development workflow.

A workaround to solve this issue is to use the `--user` option of `docker(-compose) run`:

```
$ docker-compose run --rm --user="$(id -u):$(id -g)" node yarn install
```

In the previous example, we force our local user id and primary group id both accessible in a shell context _via_ the `id` command. This little trick can also be used in a `Makefile`:

```bash
# Docker
COMPOSE              = docker-compose
COMPOSE_RUN          = $(COMPOSE) run --rm
COMPOSE_RUN_NODE     = $(COMPOSE_RUN) --user="$(id -u):$(id -g)" node

# Node
YARN                 = $(COMPOSE_RUN_NODE) yarn

build-saas: ## build Sass files to CSS
	@$(YARN) sass
.PHONY: build-saas
```



