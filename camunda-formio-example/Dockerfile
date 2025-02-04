# Faza 1: Build cu Maven
FROM maven:3.8.8-eclipse-temurin-17 AS builder

# Setăm directorul de lucru
WORKDIR /app

# Copiem fișierele necesare pentru build
COPY pom.xml .
COPY . .

# Construim aplicația (creăm fișierul .jar)
RUN mvn clean package -DskipTests

# Faza 2: Crearea imaginii finale mai mici
FROM alpine:3

# Instalăm doar OpenJDK 17 și bash
RUN apk add --no-cache openjdk17-jre-headless bash

# Setăm variabile de mediu și expunem portul
ENV PORT=8080
ENV CLASSPATH=/opt/lib
EXPOSE 8080

# Copiem doar fișierele necesare din prima fază (fără tot Maven)
COPY --from=builder /app/target/*.jar /opt/app.jar
COPY --from=builder /app/target/lib* /opt/lib/

WORKDIR /opt

# Pornim aplicația
CMD ["java", "-jar", "app.jar"]
