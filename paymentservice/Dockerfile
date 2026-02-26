FROM eclipse-temurin:21-jdk AS build
WORKDIR /app

COPY gradlew gradlew
COPY gradle gradle
COPY build.gradle settings.gradle ./
COPY src src

RUN chmod +x ./gradlew && ./gradlew clean bootJar -x test \
    && JAR_FILE=$(ls /app/build/libs/*.jar | grep -v plain | head -n 1) \
    && if [ -z "$JAR_FILE" ]; then echo "Boot JAR not found" >&2; exit 1; fi \
    && cp "$JAR_FILE" /app/app.jar

FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=build /app/app.jar app.jar

EXPOSE 8095
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
