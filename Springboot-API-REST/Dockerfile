# =====================
# STAGE 1: Build
# =====================
FROM maven:3.9.6-eclipse-temurin-17 AS builder

WORKDIR /app

COPY mvnw pom.xml ./
COPY .mvn .mvn
RUN ./mvnw -q -DskipTests dependency:go-offline

COPY src src
RUN ./mvnw -q -DskipTests package

# =====================
# STAGE 2: Runtime
# =====================
FROM eclipse-temurin:17-jre

RUN groupadd -r appgroup && useradd -r -g appgroup appuser

WORKDIR /app
COPY --from=builder /app/target/*.jar /app/app.jar

EXPOSE 8080

USER appuser

ENTRYPOINT ["java", "-jar", "/app/app.jar"]
