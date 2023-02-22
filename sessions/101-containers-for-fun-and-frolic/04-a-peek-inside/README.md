# 04: A Peek Inside



You now can pull an image from docker hub, and run it locally, using it as you would like. You'd wonder how it got
there in the first place, no?

There are several steps involved - beginning with first figuring out what you want to do. Then you create a
`Dockerfile` / `Containerfile`, build it as an image, give it a name and a tag, finally pushing it to a registry.
Once pushed, it is available to whoever you make it available to, including full public access.

## Building Images

Building images involve taking a 'base image', and adding 'layers'. Think of taking some sponge cake, putting some
cream on top, then another layer of sponge cake, more cream, some frosting, some bits of chocolate, oh yessss...

So to make a chocolate cake, you need:
- 4 cups flour
- 2 cups grated chocolate
- ...

WAIT.

So we were on docker layers. Sorry for the digression. In the context of docker, the 'base image' would typically be
a barebones environment, often linux based. The 'layers' are stuff you add to it, such as dependencies, binaries,
configuration changes, etc. For example, to build an image that ran `mongodb`, you'd add a layer that installed
`mongodb` *inside* the container. The last layer is generally the 'entrypoint' - you specify what actually runs
when the container is started. A deeper dive on this later.

## Pushing Images to a Registry

Once you've built an image, you tag it. Next, you push this to a container registry that you have access to. You
can create a free account on Docker Hub, for example. The registry provides it a unique name - thus making the image
available to all.

For all the steps above, there exist docker commands. Will delve into them later, you can of course look this up.
