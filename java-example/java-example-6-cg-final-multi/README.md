# java-example-6: Building Chainguard Maven and JDK images and referencing those in a multi-stage build
Let's take the workshop across the finish line. Instead of referencing chainguard-base in every Dockerfile and build, let's simplify our Dockerfile by building the images we need in each stage and referencing those.

Navigate to the create-cg-base-images folder:

`cd create-cg-base-images`

There are two Dockerfiles in this folder that we will use to build the two images we need for our multi-stage build. To build them execute the following commands:

`docker build . -f Dockerfile.maven -t cg-maven`

`docker build . -f Dockerfile.jdk -t cg-jdk`

Now lets build our web app:

`cd ..`

`docker build . -t java-cg-final`

Now that the app is built let's run it:

`docker run -d -p 8081:8080 java-cg-final`

Let's take a look at our app. Open up a web browser and go to:

`localhost:8081`

You should see a message confirmation:

`Chainguard Workshop - Java Example 6 - Chainguard iamges we build LETS GO!`

Before we jump to part 5, lets spin down the running container:

`docker ps`

`docker stop <container id>`
