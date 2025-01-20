# Inside a container

You can easily run commands inside a running container, as you do in any other host.

---
## Executing commands inside a container

To perform it in our running container, just execute this:

```bash
podman exec -it my-container /bin/bash
```

Your terminal prompt should by now look like this:

```bash
[jboss@24543489a4b1 ~]$
```

What you can do is directly related to what you installed inside your container, but a `curl` works in general:

```bash
curl localhost:8080/hello
```

Let’s see the `.jar` of our application:

```bash
ls /deployments/
```

We can also print the VM settings of this container:

```bash
java -XshowSettings:vm -version
```

See Maven’s installed version:

```bash
mvn -v
```

And even check the OS version details:

```bash
cat /etc/os-release
```

To go back to your previous terminal, just run:

```bash
exit
```
