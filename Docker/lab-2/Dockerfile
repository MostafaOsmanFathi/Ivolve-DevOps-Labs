FROM eclipse-temurin:latest

USER jar-runner

WORKDIR /app

COPY CalculatorMavenApp/target/calculator.jar .

#eclipse is minmal base image so it doesn't have useradd command
#workaround is to use +10000 userId so i can have non-root user 

#USER <UID>:<GID>
USER 10001:10001

CMD ["java","-jar","calculator.jar"]

EXPOSE 8080