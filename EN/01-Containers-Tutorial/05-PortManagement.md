# Port

Let’s take a look at how to enable communication of the container to the outside world by exposing and mapping ports.

---
## Exposing ports

The application running inside your container can be accessed through the port 8080, so let’s try it:

```bash
curl localhost:8080/hello
```

You should have gotten this output:

```text
curl: (7) Failed to connect to localhost port 8080: Connection refused
```

This is happening because we need to explicitly expose the ports you need, and we didn’t do it so far. So let’s fix it.

First let’s remove the container:

```bash
podman rm -f my-container
```

> **TIP:** Did you see something different?
> We used the -f flag to force the removal of the container even if it’s running. Be careful when using it!

Now we re-create the container, but exposing the port 8080:

```bash
podman run -d --name my-container -p 8080:8080 my-image
```

Let’s try to reach the application again:

```bash
curl localhost:8080/hello
```

You should now see output like this:

```text
Hello from Quarkus REST
```

This means that your application is now accessible!