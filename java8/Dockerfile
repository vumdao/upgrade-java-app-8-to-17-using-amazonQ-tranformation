FROM eclipse-temurin:8-jdk as BUILD

RUN apt-get update && \
    apt-get install -y ca-certificates curl git --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

ENV MAVEN_HOME /usr/share/maven

COPY --from=maven:3.9.6-eclipse-temurin-11 ${MAVEN_HOME} ${MAVEN_HOME}
COPY --from=maven:3.9.6-eclipse-temurin-11 /usr/local/bin/mvn-entrypoint.sh /usr/local/bin/mvn-entrypoint.sh
COPY --from=maven:3.9.6-eclipse-temurin-11 /usr/share/maven/ref/settings-docker.xml /usr/share/maven/ref/settings-docker.xml

RUN ln -s ${MAVEN_HOME}/bin/mvn /usr/bin/mvn

ARG MAVEN_VERSION=3.9.6
ARG USER_HOME_DIR="/root"
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"

ENTRYPOINT ["/usr/local/bin/mvn-entrypoint.sh"]

COPY . .

RUN mvn clean install

FROM openjdk:8-jre-slim-buster

EXPOSE 8080

COPY --from=BUILD /target/bootdemo-0.0.1-SNAPSHOT.jar /app.jar

ENTRYPOINT ["java", "-jar", "/app.jar"]