FROM gradle:8.4-jdk17 AS cache

RUN mkdir -p /home/gradle/cache_home
ENV GRADLE_USER_HOME /home/gradle/cache_home

ENV APP_HOME=/usr/app/
WORKDIR $APP_HOME

COPY build.gradle.kts $APP_HOME
COPY settings.gradle.kts $APP_HOME

RUN gradle clean build || true

FROM gradle:8.4-jdk17 AS build

COPY --from=cache /home/gradle/cache_home /home/gradle/.gradle

ENV APP_HOME=/usr/app/
WORKDIR $APP_HOME

COPY src/main $APP_HOME/src/main
COPY build.gradle.kts $APP_HOME
COPY settings.gradle.kts $APP_HOME

RUN gradle clean build

FROM eclipse-temurin:17-jdk-alpine AS run

RUN apk --no-cache add bash curl

ENV APP_HOME=/usr/app
WORKDIR $APP_HOME

COPY --from=build $APP_HOME/build/libs/*.jar $APP_HOME/app.jar

ENTRYPOINT ["sh", "-c", "java -jar ${APP_HOME}/app.jar"]
