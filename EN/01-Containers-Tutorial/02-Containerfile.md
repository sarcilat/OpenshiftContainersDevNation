# Containerfile (Dockerfile)

A Containerfile, also known as a Dockerfile, is where you’ll establish the definitions used to build a container image. It uses three main keywords/commands:

- FROM: where you’ll inform the base image used to build your own image
- COPY: where you’ll add resources (files) to your image
- CMD: where you’ll inform how to start the application

> **TIP:** Sometimes people ask when to use ADD and when to use COPY. Both of them can be used to copy files from the source to destination, but:
> - COPY: Simple copy of local files into the container
> - ADD: Will also extract local tar files and supports remote URL. It is less obvious thus it is preferable to use COPY unless you specifically need these features

---
## Creating an application to be containerized

Let’s first create a simple Java application so we can containerize it.

```bash
mvn "io.quarkus.platform:quarkus-maven-plugin:create" -DprojectGroupId="com.redhat.developers" -DprojectArtifactId="tutorial-app" -DprojectVersion="1.0-SNAPSHOT" -Dextensions=rest
cd tutorial-app
```

> **Important:** All the remaining parts of this tutorial assume that you’ll be working inside the project folder that was just created. In this case, `tutorial-app`.

To let this application be ready to be distributed, let’s package it:

```bash
mvn package
```

---
## Building a Containerfile

Create a file named `Containerfile`.

```dockerfile
cat <<EOF >Containerfile
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

EOF
```
