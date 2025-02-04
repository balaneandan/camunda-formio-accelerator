# -----------------------------------------------------------------------------
# Copyright 2025 Soft Cannery LTD.
# Licensed under the Camunda Platform Accelerator for Form.io Community License v1.0
# 
# See the LICENSE file in the project root for more details.
# -----------------------------------------------------------------------------
# Faza 1: Construirea aplicației cu Maven
FROM maven:3.8.8-eclipse-temurin-17 AS builder

WORKDIR /app

# Definim argumente pentru autentificare
ARG GITHUB_USERNAME
ARG GITHUB_TOKEN

# Copiem fișierul settings.xml existent din repository
COPY .m2/settings.xml /root/.m2/settings.xml

# Înlocuim variabilele nesetate cu argumentele corecte
RUN sed -i "s|\${repo.login}|${GITHUB_USERNAME}|g" /root/.m2/settings.xml && \
    sed -i "s|\${repo.pwd}|${GITHUB_TOKEN}|g" /root/.m2/settings.xml

# Copiem fișierele proiectului
COPY . .

# Construim aplicația cu Maven
RUN mvn clean package -DskipTests

# Faza 2: Imaginea finală pentru rulare
FROM openjdk:17-jdk-slim
WORKDIR /opt
COPY --from=builder /app/target/*.jar /opt/app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
