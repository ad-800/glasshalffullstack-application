# ### Stage 1: Build the app
# FROM openjdk:11 as build

# # Set the current working dir inside the image
# WORKDIR /app

# #
# COPY build.gradle .
# COPY build.gradle .

# --platform linux/x86_64/v8

FROM gradle:jdk17-alpine AS build
COPY --chown=gradle:gradle . /home/gradle/src
WORKDIR /home/gradle/src
RUN gradle build --no-daemon

# For some reason I need this silly debugging message or else
# the above step takes very very long. now it only takes long (220s)
RUN echo Hello I finished with build

FROM openjdk:8-jre-slim

EXPOSE 8080

RUN mkdir /app
# maybe: /home/.gradle/src/build/libs/*.jar
COPY --from=build /home/gradle/src/build/libs/*.jar /app/spring-boot-application.jar

ENTRYPOINT ["java", "-XX:+UnlockExperimentalVMOptions", "-XX:+UseCGroupMemoryLimitForHeap", "-Djava.security.egd=file:/dev/./urandom","-jar","/app/spring-boot-application.jar"]