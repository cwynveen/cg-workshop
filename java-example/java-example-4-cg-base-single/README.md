# java-example-4: Building on top of Chainguard Base - single-stage build
Time to get familiar with the chainguard-base iamge. Let's look at how we can build and run the same Java application, starting from chainguard-base.

Let's start by inspecting the chainguard-base image and look at the size and number of known vulnerabilities it has using Grype:

`grype cgr.dev/<your organization>/chainguard-base`

If you can't access your private Chainguard registry then you can use wolfi-base (free tier version of chainguard-base with only the latest versions of packages available and no FIPS modules).

`grype cgr.dev/chainguard/wolfi-base`

```bash
grype cgr.dev/chainguard-private/chainguard-base
 ✔ Loaded image                                                                                    cgr.dev/chainguard-private/chainguard-base:latest
 ✔ Parsed image                                                              sha256:326ac445ed3f55a841fd58494b8def069f22816a9c9271c259cf53e49330607f
 ✔ Cataloged contents                                                               c70cef3a1dc1b2b2706a8710892c115568399ce0cef9722f2508655a384d23c0
   ├── ✔ Packages                        [16 packages]
   ├── ✔ File digests                    [104 files]
   ├── ✔ File metadata                   [104 locations]
   └── ✔ Executables                     [27 executables]
 ✔ Scanned for vulnerabilities     [0 vulnerability matches]
   ├── by severity: 0 critical, 0 high, 0 medium, 0 low, 0 negligible
   └── by status:   0 fixed, 0 not-fixed, 0 ignored
No vulnerabilities found
```

Now let's inspect the size of the image:

`docker image inspect cgr.dev/chainguard/chainguard-base --format='{{.Size}}' | awk '{print $1/1024/1024 " MB"}'`

We can see that the image is only 5.41607 MB. Very smol.

Before we can build our Java application, we need to get familiar with using chainguard-base. Let's run the image and get familiar with searching and installing apk's.

Run the chainguard-base image in an interactive terminal:

`docker run -it cgr.dev/chainguard/wolfi-base`

Now that the container is running lets update our list of available apk packages:

`apk update`

Now let's look for Maven:

`apk search maven`

Let's install the version we want:

`apk install maven-3.9`

We are also going to need openjdk, let's look at the versions available:

`apk search openjdk`

Let's install the version we want:

`apk install openjdk-21`

Now that we understand how to install apk's on top of chainguard-base, the Dockerfile in the example should make more sense. Let's not exit out of the wolfi-base/chaingurad-base interactive terminal just yet. 

Note that the --no-cache flag in the Dockerfile makes sure that it only installs the package we want and doesn't leave behind all the other packages we don't need. 

Now lets build our web app:

`docker build . -f Dockerfile.bad -t java-cg-base-single`

Oops! Something isn't quite working. Let's troubleshoot this by going back into the wolfi-base/chainguard-base image and see what's going on. Let's try to run the java and mvn commands
```bash
mvn
The JAVA_HOME environment variable is not defined correctly,
this environment variable is needed to run this program.
8670836f80b7:/# java
/bin/sh: java: not found
8670836f80b7:/# ENV JAVA_HOME=/usr/lib/jvm/java-21-openjdk
/bin/sh: ENV: not found
8670836f80b7:/# JAVA_HOME=/usr/lib/jvm/java-21-openjdk
8670836f80b7:/# java
/bin/sh: java: not found
8670836f80b7:/# mvn
The JAVA_HOME environment variable is not defined correctly,
this environment variable is needed to run this program.
8670836f80b7:/# PATH="$JAVA_HOME/bin:$PATH"
8670836f80b7:/# mvn
[INFO] Scanning for projects...
```

Now lets try to build our web app again with these ENV variables specified:

`docker build . -t java-cg-base-single`

Now that the app is built let's run it:

`docker run -d -p 8081:8080 java-cg-base-single`

Let's take a look at our app. Open up a web browser and go to:

`localhost:8081`

You should see a message confirmation:

`Chainguard Workshop - Java Example 4 Chainguard Base Single Stage Build`

Before we jump to part 5, lets spin down the running container:

`docker ps`

`docker stop <container id>`

Let's also take a look at the built image and see if it's still vulnerability free using Grype:

```bash
grype java-cg-base-single
 ✔ Loaded image                                                                                           java-cg-base-single:latest
 ✔ Parsed image                                              sha256:6c30d1a82d7fef714b5845048d2a354dce9e6b52f92c93811748bbd54d7b9e7d
 ✔ Cataloged contents                                               027e446b3084aa7512f4cfa22dd7941c89fbb812fdaac46dab72fae86fbff312
   ├── ✔ Packages                        [251 packages]  
   ├── ✔ File digests                    [1,202 files]  
   ├── ✔ File metadata                   [1,202 locations]  
   └── ✔ Executables                     [151 executables]  
 ✔ Scanned for vulnerabilities     [8 vulnerability matches]  
   ├── by severity: 0 critical, 5 high, 3 medium, 0 low, 0 negligible
   └── by status:   8 fixed, 0 not-fixed, 0 ignored 

NAME              INSTALLED  FIXED-IN  TYPE          VULNERABILITY        SEVERITY 
commons-compress  1.25.0     1.26.0    java-archive  GHSA-4g9r-vxhx-9pgx  High      
commons-compress  1.25.0     1.26.0    java-archive  GHSA-4265-ccf5-phj5  Medium    
commons-io        2.11.0     2.14.0    java-archive  GHSA-78wr-2p64-hpwj  High      
commons-io        2.13.0     2.14.0    java-archive  GHSA-78wr-2p64-hpwj  High      
snappy            0.4        0.5       java-archive  GHSA-8wh2-6qhj-h7j9  Medium
```

Ut oh! Looks like we've introduced some vulnerabilities! Before we dig into how this happened let's jump to the next example (spoiler alert: these are build time dependencies and will dissapear in the next example)
