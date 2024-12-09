# java-example-1: building from upstream Maven
We will start by building a very simple Java web application on top of the upstream Maven image from Docker Hub.

Let's first inspect the Maven image on Docker Hub and look at the size and number of known vulnerabilities it has using Grype. The command below will pull from docker hub by default (`docker.io/library/maven:latest`):

`grype maven`

```bash
grype maven
 ✔ Vulnerability DB                [updated]
 ✔ Loaded image                                                                                                                         maven:latest
 ✔ Parsed image                                                              sha256:1c4202aef6800ae03522207cb1f2536ce7480f029c532f27234398f3ed8318f9
 ✔ Cataloged contents                                                               aeb6e05a49d5e2bc49b219e0d83d43fae13f6181629906a5dc34d06b7ba995e9
   ├── ✔ Packages                        [208 packages]
   ├── ✔ File digests                    [6,271 files]
   ├── ✔ File metadata                   [6,271 locations]
   └── ✔ Executables                     [979 executables]
 ✔ Scanned for vulnerabilities     [72 vulnerability matches]
   ├── by severity: 0 critical, 0 high, 27 medium, 30 low, 15 negligible
   └── by status:   0 fixed, 72 not-fixed, 0 ignored
```

Now let's inspect the size of the image:

`docker image inspect maven --format='{{.Size}}' | awk '{print $1/1024/1024 " MB"}'`

We can see that the image is 229.509 MB

Great, now lets build our web app:

`docker build . -t java-maven-orig`

Now that the app is built let's run it. what are each of these flags doing?

-d: Runs the container in detached mode, meaning it runs in the background rather than in the foreground of your terminal.

-p 8081:8080: Maps port 8080 on the container to port 8081 on the host machine. This means that if the application inside the container listens on port 8080, you can access it on your host machine via localhost:8081

`docker run -d -p 8081:8080 java-maven-orig`

Let's take a look at our app. Open up a web browser and go to:

`localhost:8081`

You should see a message confirmation:

`Chainguard Workshop - Java Example 1 Maven Upstream Image`

Before we jump to part 2, lets spin down the running container:

`docker ps` #show us all of the running containers on our machine

`docker stop <container id>`