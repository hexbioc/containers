# 05: Reading a Dockerfile / Containerfile


## `FROM` instruction

Building images involves starting at a base 'layer', followed by adding more 'layers' on top. Consider the additionally
added layer as a change in the base layer - this could be installing a dependency (thus creating new files), making
a configuration change (thus modifying existing files), adding commands, etc.

The `FROM` instruction specifies which base layer you want to use.


## `COPY` instruction

This instruction is used to copy files from a source to a destination. In general, this is used to copy from the local
filesystem into the container, such as the source code files. For more advanced use cases, this is also used to copy
from one 'stage' to another, more on this in a dedicated session.


## `RUN` instruction

This executes a command on the container, and creates a new layer on the image. Any files that have been created,
deleted or modified end up in the layer.


## `ENTRYPOINT` instruction

This instruction does not create a layer, rather it specifies the command with which the countainer is spawned. Typically,
this will be along the lines of `node dist/index.js`, `python main.py`, etc. Often, the `ENTRYPOINT` is coupled with the
`CMD` instruction for additional flexibility while spawning containers.

The `ENTRYPOINT` can be overridden while starting a container using the `--entrypoint` CLI argument.

## `CMD` instruction

This instruction also does not create a layer, but has multiple usages. A docker container requires either an `ENTRYPOINT`
instruction or a `CMD` instruction to start. Even if you don't provide one in your `Dockerfile`, there will be one
in the base image that you're using, and the same is used when you run the container. If a `Dockerfile` has only an
`ENTRYPOINT`, the specified command is run when the container starts. If it only has a `CMD` instead, the specified
command under `CMD` runs instead. When both are present, however, by default `docker` will run the command specified
under `ENTRYPOINT`, but also pass all the tokens listed under `CMD` to the `ENTRYPOINT` as arguments. For example,
if your `Dockerfile` contains the following:

```dockerfile
FROM some-base-image

# ... more commands

CMD ["--port", "8080"]
ENTRYPOINT ["uvicorn", "main:app"]
```

The root process of the container built with the file above would be:
```sh
uvicorn main:app --port 8080
```

The `CMD` instructions can be overridden by simply passing new arguments at the end of the `docker run` command. All
arguments passed after the image name are passed as the `CMD` for the container's `ENTRYPOINT`.

---

The commands above are primarily used to setup a simple `Dockerfile`. There are more ways to use the instructions above,
and there also exist other useful instructions. Read more about them in the
[documentation](https://docs.docker.com/engine/reference/builder/).
