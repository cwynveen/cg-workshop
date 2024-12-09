# java-example-3: multi-stage build using Chainguard images
Now let's test out a multi-stage build for our Java application. We need Maven to biuld our Java application, but we don't need it to run the application. This Dockerfile has two FROM statements. The first stage is used to build the application .jar file, and the second stage copies the .jar but only has the runtime dependencies necessary to run our application (JRE).

Let's inspect the Chainguard JRE image and look at the size and number of known vulnerabilities it has using Grype:

`grype cgr.dev/chainguard/jre:latest`

```bash
grype cgr.dev/chainguard/jre:latest
 ✔ Loaded image                                                                                                        cgr.dev/chainguard/jre:latest
 ✔ Parsed image                                                              sha256:6f6d5e026f0815dd0b367b44a4de075cf3d5ad61b240e9111ef2fa63ef150fa6
 ✔ Cataloged contents                                                               c8bc8f8ce849774ec0efac51e15e7d27918797cebb6daa23e690f48d06429073
   ├── ✔ Packages                        [41 packages]
   ├── ✔ File digests                    [1,165 files]
   ├── ✔ File metadata                   [1,165 locations]
   └── ✔ Executables                     [123 executables]
 ✔ Scanned for vulnerabilities     [0 vulnerability matches]
   ├── by severity: 0 critical, 0 high, 0 medium, 0 low, 0 negligible
   └── by status:   0 fixed, 0 not-fixed, 0 ignored
No vulnerabilities found
```

Now let's inspect the size of the image:

`docker image inspect cgr.dev/chainguard/jre --format='{{.Size}}' | awk '{print $1/1024/1024 " MB"}'`

We can see that the image is 103.652 MB. Smaller still and much less packages.

Great, now lets build our web app:

`docker build . -t java-jre-cg-multi`

Now that the app is built let's run it:

`docker run -d -p 8081:8080 java-jre-cg-multi`

Let's take a look at our app. Open up a web browser and go to:

`localhost:8081`

You should see a message confirmation:

`Chainguard Workshop - Java Example 3 Chainguard Maven Single Stage Build`

Before we jump to part 4, lets spin down the running container:

`docker ps`

`docker stop <container id>`