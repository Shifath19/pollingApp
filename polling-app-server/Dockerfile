# Stage 1: Build the application
FROM openjdk:11-jdk as build

# Set the current working directory inside the image
WORKDIR /app

# Copy Maven wrapper files
COPY mvnw .
COPY .mvn .mvn

# Copy the pom.xml file
COPY pom.xml .

# Download dependencies
RUN ./mvnw dependency:go-offline

# Copy the source code
COPY src src

# Package the application
RUN ./mvnw package -DskipTests

# Stage 2: Create the runtime image
FROM openjdk:11-jre-slim

# Set the working directory
WORKDIR /app

# Copy the packaged application from the build stage
COPY --from=build /app/target/*.jar app.jar

# Run the application
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
