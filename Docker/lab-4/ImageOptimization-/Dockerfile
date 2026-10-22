FROM maven:3.9.15-eclipse-temurin-17-alpine AS build

WORKDIR /app

COPY . .

RUN mvn package



FROM eclipse-temurin:17-jre-alpine-3.24

WORKDIR /app


COPY --from=build /app/target/demo-0.0.1-SNAPSHOT.jar .


RUN addgroup -S runner && adduser -S -G runner runner

USER runner

CMD ["java", "-jar", "demo-0.0.1-SNAPSHOT.jar"]

EXPOSE 8080