# Use the official Maven image as the base image, which comes with Maven pre-installed
FROM maven

# Set the working directory inside the container to /work for the build process
WORKDIR /work

# Copy the source code directory from the host to the container
COPY src/ src/
# Copy the Maven project configuration file (pom.xml) from the host to the container
COPY pom.xml pom.xml

# Run Maven to clean and package the project, generating the application JAR file in the target directory
RUN mvn clean package

# Dynamically retrieve the local Maven repository path and clean it up to reduce the image size
RUN REPOSITORY=$(mvn help:evaluate -Dexpression=settings.localRepository -q -DforceStdout) && rm -rf ${REPOSITORY}

# Change the working directory to /app for the final runtime setup
WORKDIR /app

# Copy the packaged JAR file from the build directory to the runtime directory
RUN cp /work/target/java-demo-app-1.0.0.jar .

# Define the entry point to run the application using the JAR file
ENTRYPOINT ["java", "-jar", "java-demo-app-1.0.0.jar"]
