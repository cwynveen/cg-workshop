# Use Chainguard base image for the build stage
FROM cg-maven:latest AS builder

# Set the working directory
WORKDIR /work

# Copy application source and Maven configuration
COPY src/ src/
COPY pom.xml pom.xml

# Build the application with Maven
RUN mvn clean package

# Set the working directory to where the JAR is built
WORKDIR /work/target

FROM cg-jdk:latest AS runner

WORKDIR /app

COPY --from=builder /work/target/java-demo-app-1.0.0.jar .

# Set the entrypoint to run the Java application
ENTRYPOINT ["java", "-jar", "java-demo-app-1.0.0.jar"]

