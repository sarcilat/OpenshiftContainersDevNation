# Running Containers

Let’s take a look at running a container, as well as listing running containers, starting/stopping containers, and checking container logs.

---
## Running a Container

As we removed the image on the previous step, we need to build it again:

```bash
podman build -t my-image .
```

Now we are ready to create a container based on this image. Just run:

```bash
podman create --name my-container my-image
```

Your container is created.

---
## Listing Containers

To see your just created container:

```bash
podman ps
```

Your output is probably something like this:

```text
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

Why isn’t your container in the list if you’ve just created it?

Because it isn’t running, and using plain `podman ps` will only list the running containers. Now try this:

```bash
podman ps -a
```

Now your output should be something like this:

```text
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
ccaadddf1c48        my-image            "java -jar /deployme…"   3 minutes ago       Created                                 my-container
```

It’s listing all containers, no matter the status. Your container is listed as `Created`.

---
## Starting Containers

To run the container you’ve just created, execute this:

```bash
podman start my-container
```

To check if it’s running, try the plain `podman ps` again:

```bash
podman ps
```

Now your output will be like this:

```text
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                          NAMES
ccaadddf1c48        my-image            "java -jar /deployme…"   8 minutes ago       Up 54 seconds       8080/tcp, 8443/tcp, 8778/tcp   my-container
```

Notice that the status now is `Up 54 seconds`.

---
## Stopping containers

To stop your container:

```bash
podman stop my-container
```

---
## Removing containers

to remove your container:

```bash
podman rm my-container
```

---
## Creating and starting a container at once

Instead of creating a container and then starting, you can do it at once:

```bash
podman run --name my-container my-image
```

You now got an output like this:

```text
INFO exec -a "java" java -Djava.util.logging.manager=org.jboss.logmanager.LogManager -cp "." -jar /deployments/quarkus-run.jar
INFO running in /deployments
__  ____  __  _____   ___  __ ____  ______
 --/ __ \/ / / / _ | / _ \/ //_/ / / / __/
 -/ /_/ / /_/ / __ |/ , _/ ,< / /_/ /\ \
--\___\_\____/_/ |_/_/|_/_/|_|\____/___/
2024-05-22 10:53:37,988 INFO  [io.quarkus] (main) tutorial-app 1.0.0-SNAPSHOT on JVM (powered by Quarkus 3.10.1) started in 0.455s. Listening on: http://0.0.0.0:8080
2024-05-22 10:53:37,989 INFO  [io.quarkus] (main) Profile prod activated.
2024-05-22 10:53:37,989 INFO  [io.quarkus] (main) Installed features: [cdi, rest, smallrye-context-propagation, vertx]
```

Notice that your terminal is attached to the container process. If you use `CTRL+C`, the container will stop.

So let’s create a detached container. First we remove this one:

```bash
podman rm my-container
```

And now we create it detached:

```bash
podman run -d --name my-container my-image
```

---
## Checking the log of a container

To see what this container is logging, use this:

```bash
podman logs my-container
```

If you want to keep following the output:

```bash
podman logs -f my-container
```

Use `CTRL+C` to stop following the log.