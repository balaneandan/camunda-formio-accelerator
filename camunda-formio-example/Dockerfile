# Copyright 2025 Your Organization Name
# Licensed under the Apache License, Version 2.0
# See LICENSE file in the project root for more information.

# Faza 1: Build cu Maven
FROM maven:3.8.8-eclipse-temurin-17 AS builder

# Setăm directorul de lucru
WORKDIR /app

# Adăugăm credențialele pentru GitHub Packages
ARG GITHUB_USERNAME
ARG GITHUB_TOKEN
RUN echo "<settings><servers><server><id>github-datakurre</id><username>${GITHUB_USERNAME}</username><password>${GITHUB_TOKEN}</password></server></servers></settings>" > /root/.m2/settings.xml

# Copiem fișierele aplicației
COPY . .

# Construim aplicația
RUN mvn clean package -DskipTests

# Faza 2: Imaginea finală pentru rulare
FROM openjdk:17-jdk-slim
WORKDIR /opt
COPY --from=builder /app/target/*.jar /opt/app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
