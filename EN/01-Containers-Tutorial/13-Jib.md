# Jib

[Jib](https://github.com/GoogleContainerTools/jib) builds optimized Docker and OCI images for your Java applications without a Docker daemon - and without deep mastery of Docker best practices.

---
## Building an Image

We’ve provided a simple Spring Boot application to containerize using Jib. Go to the application directory:

```bash
cd apps/greeting
```

Then run the following Maven command to build and push the container without using any Docker host:

```bash
./mvnw compile com.google.cloud.tools:jib-maven-plugin:3.0.0:build -Dimage=quay.io/<your-user>/greetings:1.0.0
```

Change `quay.io` for your container repository location (ie `docker.io`) and `<your-user>` for your user id.

And the output should be similar:

```text
[INFO] Using credentials from Docker config (/Users/asotobu/.docker/config.json) for adoptopenjdk:11-jre
[INFO] Using base image with digest: sha256:160242e83558e9ada7038601e1e7b4399903700a0b1685f5dced50e6ca05eced
[INFO]
[INFO] Container entrypoint set to [java, -cp, /app/resources:/app/classes:/app/libs/*, org.acme.greeting.GreetingApplication]
[INFO]
[INFO] Built and pushed image as lordofthejars/greetings:1.0.0
[INFO] Executing tasks:
[INFO] [============================  ] 91.7% complete
[INFO] > launching layer pushers
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
```

> **TIP:** No credentials are required as they are taken from `~/.docker/config.json` if they are not set.
