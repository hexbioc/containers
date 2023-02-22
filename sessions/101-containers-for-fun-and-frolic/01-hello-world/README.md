# 01: Hello World!


## Overview

Containers are pretty easy to use, and setup is generally straightforward. There are several tools and technologies
available out there that one can use for building and running containers, but to begin with you can setup `docker`.


## Installation

For Macbooks, visit the [official website](https://docs.docker.com/desktop/install/mac-install/) and install
the **right** variant for your system - depending upon whether you have an Intel chipset or an Apple one.

Linux users should be able to figure it out via their respective package managers, or can follow instructions
on the [official website](https://docs.docker.com/desktop/install/linux-install/).

### Enabling Docker

If you've installed docker desktop (rather than just the engine), you may need to enable the engine via the GUI of
docker desktop. Alternatively, such as on linux, you may have to enable the service as a daemon, and to connect to it,
you may need to add your user to the `docker` group and login again. Figure this out for your specific setup, and
verify in the next step.

### Verifying Installation

Run the following in your terminal:
```
docker ps
```

And if you get an output such as:
```
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

We should be good to go. If you get some error, you probably would have to debug it.


## Running a Container

We will delve into how containers are made and what it actually means to 'run a container' soon, at the moment
though we can get a *feel* of it by running:

```
docker run library/hello-world
```

If you see an output that goes something like:

```

Hello from Docker!
This message shows that your installation appears to be working correctly.

...
```
Congratulations! You've 'run' a container. What does that even mean? - Coming soon!
