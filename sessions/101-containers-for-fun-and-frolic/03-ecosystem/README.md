# 03: The Container Ecosystem


## Container Registries

As mentioned earlier, a container registry is like what the `npm` registry is to the Javascript ecosystem or what
PyPI is to the Python ecosystem - it is a place where people can build and share their packages, and other people
can pull from to use them on their own. In the context of containers, this 'packages' are container images,
and users can 'pull' a container image to run them locally, or in their deployment environments such as on Kubernetes,
AWS ECS, Google AppEngine, etc.

Some well known registries:

- [DockerHub](https://hub.docker.com/)
- AWS ECR - AWS's Elastic Container Registry
- Google Artifact Registry (from GCP)


## Orchestration Tools

In deployed environments, one typically does not manually execute `docker` commands to get things done. One instead
executes commands provided by *other* applications, which internally may execute a bunch of `docker` commands at once,
and thus allow you to do more with less effort. Such applications are orchestration tools.

Orchestration tools are used to run/deploy a bunch of containers and manage them, from simple tasks such as
restarting a running container to relatively complex tasks such as auto-scaling based on custom metrics. Examples
include, of course, Koooooobernetessss, but also cool stuff like AWS ECS, AWS BeanStalk, GCP AppEngine, etc.


## CNCF

TODO

## Docker Compose

Probably the simplest tool out there is `docker compose`, unlikely to be used in production unless you are either
stupid or exceptionally competent. With docker desktop, it is available out of the box. For other installations, this
may have to be installed separately.

Docker Compose requires a configuration file, conventionally named `docker-compose.yml`. It includes configuration
regarding configuring and running multiple containers, building custom images, etc. You can find the reference
specification [here](https://docs.docker.com/compose/compose-file/compose-file-v3/).

You'll find a sample `docker-compose.yml` file. You can run it using:

```bash
docker compose -f ./docker-compose.yml up -d
```

This will run `redis` and `mongodb` for you on your system ports 6380 and 27018 respectively. You or the applications
you run should be able to connect to these services once the containers have been pulled and initialized, all of
which is handled by `docker compose`.

You will also see these containers running in the output of `docker ps`:

```
CONTAINER ID   IMAGE                                         COMMAND                  CREATED          STATUS          PORTS                                           NAMES
1a824a6a6158   mongodb/mongodb-community-server:6.0.4-ubi8   "python3 /usr/local/…"   50 seconds ago   Up 50 seconds   0.0.0.0:27018->27017/tcp, :::27018->27017/tcp   03-ecosystem-mongodb-1
7f5c1ccf963c   redis:7.0.8-alpine3.17                        "docker-entrypoint.s…"   50 seconds ago   Up 50 seconds   0.0.0.0:6380->6379/tcp, :::6380->6379/tcp       03-ecosystem-redis-1
```

Make sure to stop these containers once you're done playing around, by executing:

```bash
docker compose -f ./docker-compose.yml down
```
