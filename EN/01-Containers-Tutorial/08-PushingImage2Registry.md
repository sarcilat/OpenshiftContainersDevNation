# Pushing an image to a public registry

So far you’ve been working with your container image locally. Now let’s take it to a remote registry.

---
## Tagging an image

> **Important:** Before continuing you will need to have/create a container registry account. You can sign up for a free one at [Quay.io](https://quay.io/) or the [Docker Hub](https://hub.docker.com/).

First you’ll need to tag your image accordingly to the registry and name you gave to your repository. Say for example we create a repository `myrepository`.

If you have a Docker Hub account:

```bash
podman tag my-image docker.io/myrepository/my-image
```

If using Quay.io:

```bash
docker tag my-image quay.io/myrepository/my-image
```

> **Important:** Make sure to replace `myrepository` with the name of your own repository.

> **TIP:** If you build your image already using the tag, you won’t need to do this step before pushing it. In our case it would be like this:
> ```bash
> docker build -t quay.io/myrepository/my-image .
> ```

If you now run docker images you’ll see something like this:

```text
REPOSITORY                                   TAG                 IMAGE ID            CREATED             SIZE
my-image                                     latest              a63dec174904        55 minutes ago      516MB
quay.io/myrepository/my-image                latest              a63dec174904        55 minutes ago      516MB
```

---
## Pushing an image

It’s now ready to be pushed. Before doing it, you just need to login into your registry of choice within your terminal:

With `Quay.io`:

```bash
podman login quay.io
```

With Docker Hub:

```bash
podman login
```
**Revisar este comando**

And finally you can push it, eg.:

```bash
podman push quay.io/myrepository/my-image
```

You should have an output like this:

```text
The push refers to repository [quay.io/myrepository/my-image]
abea900bc08c: Pushed
99d5cac89a03: Pushed
7b08010864ba: Pushed
90c2e42f948b: Pushed
f9ddbcc4e795: Pushed
latest: digest: sha256:de852b7869b9eff45994b676f0dd98cbfb2b8a76a8b11ca6d06e8f93c344e8c9 size: 1371
```

Now if you go to your [Quay.io](https://quay.io/) or [Docker Hub](https://hub.docker.com/) account you’ll see your image right there.
