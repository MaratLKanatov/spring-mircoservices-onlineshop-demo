FROM gradle:8.4-jdk17 AS build

ENV APP_HOME=/usr/app/
WORKDIR $APP_HOME

COPY src/main $APP_HOME/src/main
COPY build.gradle.kts $APP_HOME
COPY settings.gradle.kts $APP_HOME

RUN gradle clean build

FROM eclipse-temurin:17-jdk-alpine AS run

ENV APP_HOME=/usr/app
WORKDIR $APP_HOME

COPY --from=build $APP_HOME/build/libs/*.jar $APP_HOME/app.jar

ENTRYPOINT ["sh", "-c", "java -jar ${APP_HOME}/app.jar"]
