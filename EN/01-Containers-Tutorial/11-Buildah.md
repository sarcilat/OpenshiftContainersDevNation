# Buildah

[Buildah](https://buildah.io/) can be used to build container images compliant with the Open Container Initiative (OCI) image specification. Images can be built based on existing images from scratch and using Dockerfiles.

Buildah only offers a binary file for Linux, so to make the tutorial generic, we use the buildah container image as a standard way to run buildah.

> **TIP:** Buildah is a dockerless technology; you don’t need any Docker Host to build a container image.

---
## Preparation

In a terminal window, start the container having `buildah` installed:

```bash
podman run -it --device /dev/fuse:rw --security-opt seccomp=unconfined --security-opt apparmor=unconfined quay.io/buildah/stable:latest bash
```

---
## Creating a Container Image

Create a `Dockerfile` file, for this simple example, type `vi Dockerfile` and copy the content shown in the snippet:

```bash
FROM alpine:3.17.3

CMD ["echo", "'Hello World'"]
```

Then exit `vi` (no joke, please) by typing `:wq!`. When you are back in the shell, use buildah to build the container:

> **Tip:** remember to change `<your-user>` tag on the following commands, for the username that you created on [quay.io](https://quay.io).

```bash
buildah build -t quay.io/<your-user>/my-repository/hello:latest
```

```text
STEP 1/2: FROM alpine:3.17.3
Resolved "alpine" as an alias (/etc/containers/registries.conf.d/000-shortnames.conf)
Trying to pull docker.io/library/alpine:3.17.3...
Getting image source signatures
Copying blob f56be85fc22e done
Copying config 9ed4aefc74 done
Writing manifest to image destination
Storing signatures
STEP 2/2: CMD ["echo", "'Hello World'"]
COMMIT hello:latest
Getting image source signatures
Copying blob f1417ff83b31 skipped: already exists
Copying blob 5f70bf18a086 done
Copying config 9baf06d1b6 done
Writing manifest to image destination
Storing signatures
--> 9baf06d1b69
Successfully tagged quay.io/lordofthejars/hello:latest
9baf06d1b6916ca09ddb7a4742097d5b4be5f1286146b87d535d0ffa63fd6f90
```

The image is committed to local machine but not pushed to container registry.

```bash
buildah images
```

```bash
REPOSITORY                            TAG         IMAGE ID      CREATED        SIZE
quay.io/<your-user>/my-repository/hello           latest      9baf06d1b691  6 minutes ago  7.34 MB
docker.io/library/alpine              3.17.3      9ed4aefc74f6  2 weeks ago    7.
```

---
## Pushing a Container Image

To push the image to the container registry, first you need to log-in to the repository. On the previous steps you already do that with `podman`, but remember that at this moment you are not at your local machine, instead, you are in a container with `Buildah` deployed over there.

```bash
buildah login quay.io
```

Then you need to register your username and password of [quay.io](https://quay.io).

```text
Username: <your-user>
Password: <your-password>
Login Succeeded!
```

Now you can push the image to the container registry:

```bash
buildah push quay.io/<your-user>/my-repository/hello docker://quay.io/<your-user>/my-repository/hello:latest
```

By default, `buildah` reads credentials from `~/.docker/config.json`, but since this is a fresh container, credentials must be provided.

```text
Getting image source signatures
Copying blob b29c4850380c done
Copying blob 13ab19dd2ece done
Copying blob 4793a7e290ce done
Copying blob 4582e1897cf2 done
Copying blob cf75f156ae2e done
Copying blob bdcb28f5294e done
Copying blob 5f70bf18a086 done
Copying blob c5e55ed43ef3 done
Copying config 1de25d78a3 done
Writing manifest to image destination
Storing signatures
```

A Linux container has been created and pushed to the registry from another Linux container. You can stop the `buildah` container by typing `exit`. You can see the changes on [quay.io](https://quay.io) site