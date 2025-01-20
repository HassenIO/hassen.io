---
title: "Debug a Docker build failure"
date: 2022-05-08T11:24:00+01:00
draft: false
summary: Tips on how to debug docker build failure.
---

![A sinked ship. Photo by Emma Watson on Unsplash](/2022/05/unsplash-sink-ship.jpg "A sinked ship")

Hello world!

There are some issues that looks simple, stupid, however they take hours if not days to fix.

One example I had recently was with a failing docker build. "It should work! It must work!"

However, it took me I guess an hour to figure out what was wrong.

> tl;dr: Prepend your docker build with `DOCKER_BUILDKIT=0` and enter the last working image cache.

## Buildkit

Buildkit is a Docker feature that improves the Docker backend to build images. I can't do better than the official Docker documentation, so please have a look [here](https://docs.docker.com/build/buildkit/).

The major improvenets I can list are improved build speed because the Docker Engine can solve the build graph in parallel: The new build uses a fully concurrent build graph solver. That's why we have much faster builds.

However, with the new user experience, we no longer have the hashes of the built image layers. That's a problem, because each image layer can be accessed, so the idea is to find the last built image, and run a container of that image to find issues.

## How to find the issue?

The image hashes was an information available in the legacy builder. It turns out that we can temporarily build using the legacy builder: By appending our docker build with `DOCKER_BUILDKIT=0`, which disables Buildkit, forcing the engine to use the legacy builder.

In our example, we have:

```bash
```

Here we see that the last successful layer has the hash ``.

We can run into that image using `docker run` like the following:

```bash
```

## Final words

Cheers,

Hassen
