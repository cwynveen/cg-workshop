# java-example-2: building from Chainguard's Maven image
Now let's build that same simple Java web application on top of the Chainguards Maven image.

Let's inspect the Chainguard Maven image and look at the size and number of known vulnerabilities it has using Grype:

`grype cgr.dev/chainguard/maven:latest`

```bash
grype cgr.dev/chainguard/maven:latest
 ✔ Loaded image                                                                                                      cgr.dev/chainguard/maven:latest
 ✔ Parsed image                                                              sha256:cb22a936c3138a0a2f6073ea0066239e9015ceb158e82ab7b609cf7c4ba768de
 ✔ Cataloged contents                                                               4a2d917ee1e289820d920d302b33f9146bb34c16a786f06dbea4b070aca36c58
   ├── ✔ Packages                        [94 packages]
   ├── ✔ File digests                    [1,259 files]
   ├── ✔ File metadata                   [1,259 locations]
   └── ✔ Executables                     [147 executables]
 ✔ Scanned for vulnerabilities     [0 vulnerability matches]
   ├── by severity: 0 critical, 0 high, 0 medium, 0 low, 0 negligible
   └── by status:   0 fixed, 0 not-fixed, 0 ignored
```

Now let's inspect the size of the image:

`docker image inspect cgr.dev/chainguard/maven:latest --format='{{.Size}}' | awk '{print $1/1024/1024 " MB"}'`

We can see that the image is 111..947 MB. Half the size of the 229 MB Maven image from Docker hub with half the number of packages!

Great, now lets build our web app:

`docker build . -t java-maven-cg-single`

Now that the app is built let's run it:

`docker run -d -p 8081:8080 java-maven-cg-single`

Let's take a look at our app. Open up a web browser and go to:

`localhost:8081`

You should see a message confirmation:

`Chainguard Workshop - Java Example 2 Chainguard Maven Single Stage Build`

Before we jump to part 3, lets spin down the running container:

`docker ps`

`docker stop <container id>`