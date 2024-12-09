# Getting familiar with Chainguard images and chainguard-base
This repo will get you hands on with Chainguard images and multi-stage builds with a simple Java web-application example.

# Overview of the examples we will explore in this lab
* java-example-1-orig
Building a simple Java web-application using the Maven image from Docker Hub

* java-example-2-cg-single
Building the same Java application using the Chainguard Maven image

* java-example-3-cg-multi
Building the application using a multi-stage build. The Chainguard Maven image is used to build the application. We then copy the compiled .jar file into our final stage, which uses a more minimal image (JRE)

* java-example-4-cg-base-single
In this section we get hands on using the chainguard-base image and learn how we can add packages needed to build our Java application. This example uses a single stage build to install Maven and the Java development tools needed to build our application

* java-example-5-cg-base-multi
This example introduces the multi-stage build for our application. Both stages start with chainguard-base and install only the necessary components to build and run the application

* java-example-6-cg-final-multi
In this final example, we build the two images that are needed in the multi-stage build locally and reference those in the Dockerfile to simplify things

To get started, clone this repo locally:

`git clone https://github.com/cwynveen/cg-workshop.git`

Let's dive straight in with example 1:

`cd java-example`

`cd java-example-1-orig`
