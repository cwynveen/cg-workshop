# Use Chainguard base image for the build stage
FROM cgr.dev/chainguard-private/chainguard-base AS builder

# Set the working directory
WORKDIR /work

# Install Maven and dependencies using apk
RUN apk add --no-cache maven-3.9 openjdk-21


# Explicitly set JAVA_HOME for the OpenJDK installed by apk
ENV JAVA_HOME=/usr/lib/jvm/java-21-openjdk
ENV PATH="$JAVA_HOME/bin:$PATH"
RUN echo "JAVA_HOME=$JAVA_HOME" && java -version && javac -version

# Copy application source and Maven configuration
COPY src/ src/
COPY pom.xml pom.xml

# Build the application with Maven
RUN mvn clean package

# Set the working directory to where the JAR is built
WORKDIR /work/target

# Set the entrypoint to run the Java application
ENTRYPOINT ["java", "-jar", "java-demo-app-1.0.0.jar"]
