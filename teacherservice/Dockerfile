FROM maven:3.9-eclipse-temurin-17

WORKDIR /app

COPY . .

RUN mvn clean package -DskipTests && cp target/*.jar app.jar

EXPOSE 5002

CMD ["java", "-jar", "app.jar"]