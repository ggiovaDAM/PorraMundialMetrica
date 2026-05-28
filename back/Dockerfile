# Build stage
FROM gradle:8-jdk21 AS build
WORKDIR /app
COPY build.gradle.kts settings.gradle.kts gradle* ./
COPY gradle ./gradle
RUN gradle build --no-daemon -x test --no-build-cache || true
COPY . .
RUN gradle bootJar --no-daemon

# Run stage
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
RUN addgroup -g 1000 spring && adduser -D -u 1000 -G spring spring
COPY --from=build --chown=spring:spring /app/build/libs/*.jar app.jar
USER spring
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
