# 02: Running Containers Locally


## Overview

We will run a container locally, and interact with it.

## Running PostgreSQL


### Pulling the Image

To run PostgreSQL on your system, we will be pulling the official PostgreSQL image from
[dockerhub](https://hub.docker.com/_/postgres). Run this command:

```bash
docker pull postgres:15.2-alpine3.17
```
You should see docker report that its pulling a bunch of 'layers' by downloading them, finally reporting
completion. This could take a while, depending upon your internet bandwidth.


### Running DB

'Pulling' an image is akin to downloading an installer, and the next step typically then is to
'install', which thanks to docker, is already done. Finally, to 'run', we run:

```bash
docker run \
    -d \
    --name pgdemo \
    -p '5432:5432' \
    -e 'POSTGRES_USER=demouser' \
    -e 'POSTGRES_PASSWORD=scarypassword' \
    -e 'POSTGRES_DB=demodb' \
    postgres:15.2-alpine3.17

```

Verify that the container is running with:
```bash
docker ps
```

This will show you the container that you're running, with an output similar to:

```
CONTAINER ID   IMAGE                      COMMAND                  CREATED          STATUS          PORTS                                       NAMES
e23906a90a81   postgres:15.2-alpine3.17   "docker-entrypoint.s…"   12 seconds ago   Up 12 seconds   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   pgdemo
```

Notice the `pgdemo` name under the `NAMES` column - yup, this is what you ran.

## Interacting with PostgreSQL

If you have `psql` setup, you can now connect to the DB you have running with the command:

```bash
psql -h localhost -p 5432 -U demouser -d demodb
```

If you don't, you can connect *via* docker:

```bash
docker exec -it pgdemo psql -h localhost -p 5432 -U demouser -d demodb
```

Either way, you should receive a prompt for password, to which you'd of course enter
`scarypassword` - and you're in! You should see a `psql` prompt that looks like:

```
demodb=#
```

Lets create a table, run this SQL query, obviously by copying from here:

```sql
CREATE TABLE DemoTable(
    id          INT         PRIMARY KEY,
    name        TEXT        NOT NULL
);
```

Check if its created:
```
\d DemoTable;
```

You should see the table's description and structure printed out:

```
             Table "public.demotable"
 Column |  Type   | Collation | Nullable | Default 
--------+---------+-----------+----------+---------
 id     | integer |           | not null | 
 name   | text    |           | not null | 
Indexes:
    "demotable_pkey" PRIMARY KEY, btree (id)
```

Congratulations! You've now just run and interacted with a container. Feel free to take some time
out to call your parents and tell them about this new feat that you've achieved.


## What just happened?!

If you followed along like a good student, you'd have roughly the same outputs as I've described above.
Lets break the stuff down a bit:

### The `docker pull` command

This downloaded an 'image' package from docker's official 'container registry'. More on what an image
or a registry is later, but briefly, think of it akin to `npm` and `npm`'s registry, or `pip` and PyPI,
or a package manager interacting with a internet based repository of publicly available packages.

### The `docker run` command

Will break this down in the comments:

```bash
# The `docker run` command is used to 'run', that is execute a container. In this case, we used it
# to start a container that had PostgreSQL inside it - so that we could connect and interact with
# PostgreSQL.
docker run \

    # Here we gave a name to the container, so that we could identify it later
    --name pgdemo \

    # Here we specified that we want to forward the containers port 5432 to the local system port
    # 5432, so that we can connect to it locally. We need this to connect to the DB using `psql`
    -p '5432:5432' \

    # This is an environment variable passed to the container, which tells PostgreSQL the default
    # username to use
    -e 'POSTGRES_USER=demouser' \

    # This environment variable is the default user's password
    -e 'POSTGRES_PASSWORD=scarypassword' \

    # The default DB name to create
    -e 'POSTGRES_DB=demodb' \

    # Finally, the name of the container image that we want to run. This can be broken down into
    # two parts as <image-name>:<image-tag>. The image name is typically the application inside
    # the container, while the 'tag' is similar to the image 'version', in this case the tag says
    # that the image will run PostgreSQL version 15.2, with the base operating system as Alpine
    # Linux 3.17. Tag naming is a convention, different images could have differently named tags.
    postgres:15.2-alpine3.17
```

### The `docker exec` command

```bash
# The docker exec command is used to execute a command INSIDE a running container. In this case, we
# used it to run the `psql` command, which is a PostgreSQL client, to connect to the running PostgreSQL
# database. This was possible because the PostgreSQL container had the `psql` client installed inside it.
docker exec \

    # The -t option says that we want a TTY session, and the -i option says that we need the session to be
    # 'interactive'. 'pgdemo' here refers to the name of the container we want to execute `psql` in, and this
    # is the same name that we specified in the `docker run` command.
    -it pgdemo \

    # This is finally the command that will run inside the container.
    psql -h localhost -p 5432 -U demouser -d demodb
```

## Stopping the Container

Whenever you start a container, it gets assigned an ID. You can retrieve it using `docker ps`. For example, when I
run `docker ps`, I get:

```
CONTAINER ID   IMAGE                      COMMAND                  CREATED              STATUS              PORTS                                       NAMES
e23906a90a81   postgres:15.2-alpine3.17   "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   pgdemo
```

Here, `e23906a90a81` is the container ID. To be accurate, this is the prefix of the container ID; the whole container
ID is typically much longer.

A running container can be stopped:

```bash
docker stop e23906a90a81
```

Verify that it is no longer running with `docker ps`, which should not list the `pgdemo` container anymore.
