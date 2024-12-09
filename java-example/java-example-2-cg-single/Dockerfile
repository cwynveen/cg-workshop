# Use the Chainguard Maven image as the base image for the build process
FROM cgr.dev/chainguard/maven:latest AS builder

# Set the working directory inside the container to /work for the build process
WORKDIR /work

# Copy the source code directory from the host to the container
COPY src/ src/
# Copy the Maven project configuration file (pom.xml) from the host to the container
COPY pom.xml pom.xml

# Run Maven to clean and package the project, generating the application JAR file in the target directory
RUN mvn clean package

# Change the working directory to /app for the runtime setup
WORKDIR /app

# Copy the packaged JAR file from the build directory to the runtime directory
RUN cp /work/target/java-demo-app-1.0.0.jar .

# Define the entry point to run the application using the JAR file
ENTRYPOINT ["java", "-jar", "java-demo-app-1.0.0.jar"]
