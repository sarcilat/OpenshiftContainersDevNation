# Skopeo

[Skopeo](https://github.com/containers/skopeo) is a command line utility that performs operations on container images and image repositories. One of the big advantages of `Skopeo` is not requiring docker installed, it works with no `Docker host`, or requiring `root` permissions.

The operations you can do with Skopeo are:

- Copying an image from and to various storage mechanisms. For example you can copy images from one registry to another, without requiring privilege.
- Inspecting a remote image showing its properties including its layers, without requiring you to pull the image to the host.
- Deleting an image from an image repository.
- Syncing an external image repository to an internal registry for air-gapped deployments.

When required by the repository, Skopeo can pass the appropriate credentials and certificates for authentication.

---
## Installing skopeo

On Mac you can use the command:

```bash
brew install skopeo
```

---
## Inspecting a Container

With Skopeo, you can inspect container properties without having to download the container locally. In a terminal window, run the following command:

```bash
skopeo inspect docker://quay.io/rhdevelopers/quarkus-demo:v1
```

```json
{
    "Name": "quay.io/rhdevelopers/quarkus-demo",
    "Digest": "sha256:0af67e41fa74bc87b12c2e7d6a90cd4cb93bd63ba3c64d5de5c1459beb7e091e",
    "RepoTags": [
        "v1"
    ],
    "Created": "2020-04-09T16:54:58.744044835Z",
    "DockerVersion": "1.13.1",
    "Labels": {
        "architecture": "x86_64",
        "authoritative-source-url": "registry.access.redhat.com",
        "build-date": "2020-03-31T14:51:49.719962",
        "com.redhat.build-host": "cpt-1002.osbs.prod.upshift.rdu2.redhat.com",
        "com.redhat.component": "ubi8-minimal-container",
        "com.redhat.license_terms": "https://www.redhat.com/en/about/red-hat-end-user-license-agreements#UBI",
        "description": "The Universal Base Image Minimal is a stripped down image that uses microdnf as a package manager. This base image is freely redistributable, but Red Hat only supports Red Hat technologies through subscriptions for Red Hat products. This image is maintained by Red Hat and updated regularly.",
        "distribution-scope": "public",
        "io.k8s.description": "The Universal Base Image Minimal is a stripped down image that uses microdnf as a package manager. This base image is freely redistributable, but Red Hat only supports Red Hat technologies through subscriptions for Red Hat products. This image is maintained by Red Hat and updated regularly.",
        "io.k8s.display-name": "Red Hat Universal Base Image 8 Minimal",
        "io.openshift.expose-services": "",
        "io.openshift.tags": "minimal rhel8",
        "maintainer": "Red Hat, Inc.",
        "name": "ubi8-minimal",
        "release": "409",
        "summary": "Provides the latest release of the minimal Red Hat Universal Base Image 8.",
        "url": "https://access.redhat.com/containers/#/registry.access.redhat.com/ubi8-minimal/images/8.1-409",
        "vcs-ref": "8c3c7acc321ed054dded6e6e13b5c09c043f42dc",
        "vcs-type": "git",
        "vendor": "Red Hat, Inc.",
        "version": "8.1"
    },
    "Architecture": "amd64",
    "Os": "linux",
    "Layers": [
        "sha256:b26afdf22be4e9c30220796780a297b91549a3b3041b6fdcbda71bf48a6912e7",
        "sha256:218f593046abe6e9f194aed3fc2a2ad622065d6800175514dffa55dfce624b56",
        "sha256:e039cd5e7c31c4ed22fbd63342cec9b36d4e8cc11f795b89d69b6d18d446965c",
        "sha256:063b9adac13a646f34cdfbd5d168ea3bf33e0468119ba4ebc00dbbfade03c9d9",
        "sha256:dbbd5482c1ea09b8a97637de2fc2742a8cbb5fcbf6a7ba83313c881c55203587"
    ],
    "LayersData": [
        {
            "MIMEType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
            "Digest": "sha256:b26afdf22be4e9c30220796780a297b91549a3b3041b6fdcbda71bf48a6912e7",
            "Size": 34668948,
            "Annotations": null
        },
        {
            "MIMEType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
            "Digest": "sha256:218f593046abe6e9f194aed3fc2a2ad622065d6800175514dffa55dfce624b56",
            "Size": 1529,
            "Annotations": null
        },
        {
            "MIMEType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
            "Digest": "sha256:e039cd5e7c31c4ed22fbd63342cec9b36d4e8cc11f795b89d69b6d18d446965c",
            "Size": 127,
            "Annotations": null
        },
        {
            "MIMEType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
            "Digest": "sha256:063b9adac13a646f34cdfbd5d168ea3bf33e0468119ba4ebc00dbbfade03c9d9",
            "Size": 9395479,
            "Annotations": null
        },
        {
            "MIMEType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
            "Digest": "sha256:dbbd5482c1ea09b8a97637de2fc2742a8cbb5fcbf6a7ba83313c881c55203587",
            "Size": 93,
            "Annotations": null
        }
    ],
    "Env": [
        "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
        "container=oci"
    ]
}
```

---
## Copying a Containers

Skopeo lets you copy containers from one container registry to anotehr container registry without a Docker host. You can copy containers from different sources to different sources, for example, from remote container registry to local directory, or another remote container registry, or local container registry.

Let’s pull a remote container to a local directory. In a terminal window, run the following command:

```bash
skopeo copy docker://quay.io/rhdevelopers/quarkus-demo:v1 dir:./quakurs-demo-skopeo
```

```text
Getting image source signatures
Copying blob 063b9adac13a done
Copying blob dbbd5482c1ea done
Copying blob 218f593046ab done
Copying blob b26afdf22be4 done
Copying blob e039cd5e7c31 done
Copying config e40635aea7 done
Writing manifest to image destination
Storing signatures
```

The command created the `quakurs-demo-skopeo` directory with container content stored inside. In `Linux/MacOS` list the folder’s content by running:

```bash
ls -alh ./quakurs-demo-skopeo
```

```text
total 86128
drwxr-xr-x  10 asotobu  staff   320B Apr 11 16:36 .
drwxr-xr-x  25 asotobu  staff   800B Apr 11 16:36 ..
-rw-r--r--   1 asotobu  staff   9.0M Apr 11 16:36 063b9adac13a646f34cdfbd5d168ea3bf33e0468119ba4ebc00dbbfade03c9d9
-rw-r--r--   1 asotobu  staff   1.5K Apr 11 16:36 218f593046abe6e9f194aed3fc2a2ad622065d6800175514dffa55dfce624b56
-rw-r--r--   1 asotobu  staff    33M Apr 11 16:36 b26afdf22be4e9c30220796780a297b91549a3b3041b6fdcbda71bf48a6912e7
-rw-r--r--   1 asotobu  staff    93B Apr 11 16:36 dbbd5482c1ea09b8a97637de2fc2742a8cbb5fcbf6a7ba83313c881c55203587
-rw-r--r--   1 asotobu  staff   127B Apr 11 16:36 e039cd5e7c31c4ed22fbd63342cec9b36d4e8cc11f795b89d69b6d18d446965c
-rw-r--r--   1 asotobu  staff   5.3K Apr 11 16:36 e40635aea714ab8863092445e55f83c59575c683057c93aa9a8bd8ef2ff234ea
-rw-r--r--   1 asotobu  staff   1.3K Apr 11 16:36 manifest.json
-rw-r--r--   1 asotobu  staff    33B Apr 11 16:36 version
```

In the previous case, we copied container from a remote registry to local directory. Let’s copy a container from a remote registry to a local registry (the one running inside Docker/Podman).

Having a Docker Host running in local machine, run the following command:

```bash
skopeo copy docker://quay.io/rhdevelopers/quarkus-demo:v1 docker-daemon:docker.io/rhdevelopers/quarkus-demo:skopeo
```

```text
Getting image source signatures
Copying blob b26afdf22be4 done
Copying blob 218f593046ab done
Copying blob e039cd5e7c31 done
Copying blob 063b9adac13a done
Copying blob dbbd5482c1ea done
Copying config e40635aea7 done
Writing manifest to image destination
Storing signatures
```

Now, instead of using dir in the destination part, we use the `docker-daemon` destination to set the local registry. If you run the `podman images` command, you’ll see the image downloaded with the `skopeo` tag:

```bash
podman images
```

```text
rhdevelopers/quarkus-demo                              skopeo               e40635aea714   3 years ago     135MB
```

---
## Copying Between Registries

To copy between registries, you need to use the `podman` prefix. First of all, let’s start a container registry inside Docker Host. Run the following command in a terminal window:

```bash
podman run --rm -ti -p 5000:5000 --restart=always --name registry registry:2
```

In a new terminal window, run the `copy` command setting origin to quay.io and the destination, the registry created in the previous step:

```bash
skopeo copy docker://quay.io/rhdevelopers/quarkus-demo:v1 docker://localhost:5000/rhdevelopers/quarkus-demo:skopeo --dest-tls-verify=false
```

```text
Getting image source signatures
Copying blob 063b9adac13a done
Copying blob dbbd5482c1ea done
Copying blob b26afdf22be4 done
Copying blob e039cd5e7c31 done
Copying blob 218f593046ab done
Copying config e40635aea7 done
Writing manifest to image destination
Storing signatures
```

If you inspect the log lines of the registry container, you’ll see that the image has been stored inside the registry:

```text
172.17.0.1 - - [12/Apr/2023:13:29:37 +0000] "PUT /v2/rhdevelopers/quarkus-demo/blobs/uploads/896bdd86-13c1-4ab3-896f-81aad8a4ece7?_state=uNU2KUJnK1S9oa2Pc0hn7BOp4u6ryv0Mlc_3w-KrG1F7Ik5hbWUiOiJyaGRldmVsb3BlcnMvcXVhcmt1cy1kZW1vIiwiVVVJRCI6Ijg5NmJkZDg2LTEzYzEtNGFiMy04OTZmLTgxYWFkOGE0ZWNlNyIsIk9mZnNldCI6NTQzNSwiU3RhcnRlZEF0IjoiMjAyMy0wNC0xMlQxMzoyOTozN1oifQ%3D%3D&digest=sha256%3Ae40635aea714ab8863092445e55f83c59575c683057c93aa9a8bd8ef2ff234ea HTTP/1.1" 201 0 "" "skopeo/1.11.1"
time="2023-04-12T13:29:37.825828496Z" level=info msg="response completed" go.version=go1.16.15 http.request.contenttype="application/octet-stream" http.request.host="localhost:5000" http.request.id=3733f053-2a4f-4493-bbc7-b94e377d771a http.request.method=PUT http.request.remoteaddr="172.17.0.1:64884" http.request.uri="/v2/rhdevelopers/quarkus-demo/blobs/uploads/896bdd86-13c1-4ab3-896f-81aad8a4ece7?_state=uNU2KUJnK1S9oa2Pc0hn7BOp4u6ryv0Mlc_3w-KrG1F7Ik5hbWUiOiJyaGRldmVsb3BlcnMvcXVhcmt1cy1kZW1vIiwiVVVJRCI6Ijg5NmJkZDg2LTEzYzEtNGFiMy04OTZmLTgxYWFkOGE0ZWNlNyIsIk9mZnNldCI6NTQzNSwiU3RhcnRlZEF0IjoiMjAyMy0wNC0xMlQxMzoyOTozN1oifQ%3D%3D&digest=sha256%3Ae40635aea714ab8863092445e55f83c59575c683057c93aa9a8bd8ef2ff234ea" http.request.useragent="skopeo/1.11.1" http.response.duration=7.510274ms http.response.status=201 http.response.written=0
time="2023-04-12T13:29:37.927391556Z" level=info msg="response completed" go.version=go1.16.15 http.request.contenttype="application/vnd.docker.distribution.manifest.v2+json" http.request.host="localhost:5000" http.request.id=98b957a4-42c3-449e-bf73-2d7c5473d1d6 http.request.method=PUT http.request.remoteaddr="172.17.0.1:64886" http.request.uri="/v2/rhdevelopers/quarkus-demo/manifests/skopeo" http.request.useragent="skopeo/1.11.1" http.response.duration=7.479779ms http.response.status=201 http.response.written=0
172.17.0.1 - - [12/Apr/2023:13:29:37 +0000] "PUT /v2/rhdevelopers/quarkus-demo/manifests/skopeo HTTP/1.1" 201 0 "" "skopeo/1.11.1"
```

Shut down the container registry by stopping the podman run process.

---
## Deleting a Container

Skopeo can delete containers from repositories without requiring to have a Docker host. This operation is not always supported and requires credentials as it’s an operation that modifies the repository.

```bash
skopeo delete docker://<repository>/imagename:latest
```
