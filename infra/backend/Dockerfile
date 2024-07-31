#build stage
FROM maven:3.8.7-openjdk-18 AS build
WORKDIR /build
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

# Runtime stage
FROM amazoncorretto:17
ARG PROFILE=dev
ARG APP_VERSION=1.0.0

#define few things
WORKDIR /app
COPY --from=build /build/target/clinicalapi-1.0.0.jar /app/
RUN chmod +x /app/clinicalapi-1.0.0.jar

EXPOSE 8088

ENV DB_URL=jdbc:postgresql://postgres-sql-clinicalapp:5432/clinicaldb
ENV JAVA_VERSION=${APP_VERSION}
ENV ACTIVE_PROFILE=${PROFILE}

# Définir le point d'entrée
CMD ["java", "-jar", "-Dspring.datasource.url=${DB_URL}", "-Dspring.profiles.active=${ACTIVE_PROFILE}", "/app/clinicalapi-1.0.0.jar"]