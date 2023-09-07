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


The commands above are primarily used to setup a simple `Dockerfile`. There are more ways to use the instructions above,
and there also exist other useful instructions. Read more about them in the
[documentation](https://docs.docker.com/engine/reference/builder/).
