# java-example-5: Building on top of Chainguard Base - multi-stage build
Let's get rid of unnecessary components in our runtime image by introducing another stage that doesn't have Maven.

The second stage looks similar to the first, except we are removing the install of Maven, since Maven is used for building the Java application, but not needed for running the application.

Now lets build our web app:

`docker build . -t java-cg-base-multi`

Now that the app is built let's run it:

`docker run -d -p 8081:8080 java-cg-base-multi`

Let's take a look at our app. Open up a web browser and go to:

`localhost:8081`

You should see a message confirmation:

`Chainguard Workshop - Java Example 5 Chainguard Base Multi-Stage Build`

Before we jump to part 5, lets spin down the running container:

`docker ps`

`docker stop <container id>`

Ok, now let's run a Grype scan on this image and see if we still have those vulnerabilities from before:

```bash
grype java-cg-base-multi
 ✔ Loaded image                                                                                             java-cg-base-multi:latest
 ✔ Parsed image                                               sha256:ac5f9f2968d36745ab1aea2d0e01765d3f723fcab9709d1c3bc0ff18bc6e79ec
 ✔ Cataloged contents                                                d73a2b4f355d2f1b1bdcf3d36bf5d950b63002eaa204f082de84037aca1a5827
   ├── ✔ Packages                        [83 packages]  
   ├── ✔ File digests                    [1,034 files]  
   ├── ✔ File metadata                   [1,034 locations]  
   └── ✔ Executables                     [151 executables]  
 ✔ Scanned for vulnerabilities     [0 vulnerability matches]  
   ├── by severity: 0 critical, 0 high, 0 medium, 0 low, 0 negligible
   └── by status:   0 fixed, 0 not-fixed, 0 ignored 
No vulnerabilities found
```

Nice! Since we only copied over the compiled application into the second stage, we got rid of the build time dependencies that our application needed
