# Partimos de la imagen oficial de Jenkins LTS con JDK 21
FROM jenkins/jenkins:lts-jdk21

# Cambiamos a usuario root para poder instalar cosas
USER root

## PASO 1: INSTALAR DEPENDENCIAS BÁSICAS (curl, gnupg2, etc.)
RUN apt-get update && \
    # ELIMINADO: software-properties-common
    apt-get install -y curl gnupg2 lsb-release ca-certificates apt-transport-https && \
    # Limpiamos las listas de apt después del update/install para reducir el tamaño
    rm -rf /var/lib/apt/lists/*

## PASO 2: INSTALAR NODE.JS Y NPM
RUN apt-get update && \
    curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g npm@latest && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

## PASO 3: INSTALAR DOCKER CLI (Docker-in-Docker)
RUN curl -fsSL https://get.docker.com | bash

# Verificación opcional (la mantendremos)
RUN node -v && npm -v && docker -v

# Regresamos al usuario Jenkins (seguridad)
USER jenkins