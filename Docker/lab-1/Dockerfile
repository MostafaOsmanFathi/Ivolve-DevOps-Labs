FROM maven:3.10.0-eclipse-temurin-17

WORKDIR /app

COPY CalculatorMavenApp .

RUN mvn package

CMD ["java", "-jar", "target/calculator.jar"]

EXPOSE 8080