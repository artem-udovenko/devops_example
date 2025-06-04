# Build stage
FROM maven:3.8.6-openjdk-18 AS build
COPY . .

RUN mvn clean package -DskipTests && \
    mvn dependency:purge-local-repository -DactTransitively=false -DreResolve=false && \
    rm -rf /root/.m2/repository/*

# Package stage
FROM openjdk:18-alpine

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --from=build --chown=appuser:appgroup /target/*.jar /app.jar

RUN find / -type f -name '*.cache' -delete

USER appuser

EXPOSE 8080
ENTRYPOINT ["java","-jar","/app.jar"]
