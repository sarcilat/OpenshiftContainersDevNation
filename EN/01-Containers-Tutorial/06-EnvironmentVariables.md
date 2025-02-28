# Environment variables

Environment variables keep your app secure, flexible and organized. Let’s take a look at how to pass environment variables to containers.

---
## Using an environment variable with your container

Let’s edit the Java application you created using Quarkus:

```bash
code .
```

Now edit the `HelloResource.java` class like this:

```java
package com.redhat.developers;

import java.util.Optional;

import org.eclipse.microprofile.config.inject.ConfigProperty;

import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.Produces;
import jakarta.ws.rs.core.MediaType;

@Path("/hello")
public class HelloResource {

    @ConfigProperty(name = "config")
    Optional<String> config;

    @GET
    @Produces(MediaType.TEXT_PLAIN)
    public String hello() {
        return config.orElse("no config");
    }
}
```

Notice that instead of just printing `hello`, it will print the content of a `ConfigProperty` or print `no config` if the config doesn’t exist.

Now, back to terminal, package your project:

```bash
mvn package -DskipTests=true
```

Rebuild our image to get the new version of the application:

```bash
podman build -t my-image .
```

Remove your running container:

```bash
podman rm -f my-container
```

And run the new one:

```bash
podman run -d --name my-container -p 8080:8080 my-image
```

Now let’s call the application:

```bash
curl localhost:8080/hello
```

You now got this output.

```text
no config
```

The `no config` output happens because we didn’t really create the environment variable. Let’s fix it.

Edit your Containerfile like this (or simply add the `ENV config=dockerfile` line):

```dockerfile
FROM registry.access.redhat.com/ubi9/openjdk-21-runtime:1.18-4

ENV LANGUAGE='en_US:en'

COPY --chown=185 target/quarkus-app/lib/ /deployments/lib/
COPY --chown=185 target/quarkus-app/*.jar /deployments/
COPY --chown=185 target/quarkus-app/app/ /deployments/app/
COPY --chown=185 target/quarkus-app/quarkus/ /deployments/quarkus/

EXPOSE 8080
USER 185
ENV JAVA_OPTS="-Djava.util.logging.manager=org.jboss.logmanager.LogManager"
ENV JAVA_APP_JAR="/deployments/quarkus-run.jar"

ENV config=containerfile
```

Let’s rebuild our image, re-create the container and call it again:

```bash
podman build -t my-image .
podman rm -f my-container
podman run -d --name my-container -p 8080:8080 my-image
curl localhost:8080/hello
```

Now your output is:

```text
containerfile
```

Finally, let’s replace the variable’s content. First we remove the container:

```bash
podman rm -f my-container
```

And then we re-create it passing a new value for the environment variable:

```bash
podman run -d --name my-container -p 8080:8080 -e config=container my-image
```

Then we call the application again:

```bash
curl localhost:8080/hello
```

And the output is:

```bash
container
```