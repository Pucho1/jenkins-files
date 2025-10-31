# Partimos de la imagen oficial de Jenkins LTS con JDK 21
FROM jenkins/jenkins:lts-jdk21

# Cambiamos a usuario root para instalar dependencias del sistema
USER root

# ------------------------------------------------------------------------
# 🧩 PASO 1: INSTALAR DEPENDENCIAS BÁSICAS
# ------------------------------------------------------------------------
RUN apt-get update && \
    apt-get install -y curl gnupg2 lsb-release ca-certificates apt-transport-https wget && \
    rm -rf /var/lib/apt/lists/*

# ------------------------------------------------------------------------
# 🧩 PASO 2: INSTALAR NODE.JS 20 Y NPM
# ------------------------------------------------------------------------
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g npm@latest && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# ------------------------------------------------------------------------
# 🧩 PASO 3: INSTALAR SOLO EL CLIENTE DE DOCKER (NO EL DAEMON)
#     → Esto evita conflictos y funciona con el socket del host (/var/run/docker.sock)
# ------------------------------------------------------------------------
RUN install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc && \
    chmod a+r /etc/apt/keyrings/docker.asc && \
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \
      https://download.docker.com/linux/debian \
      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" \
      > /etc/apt/sources.list.d/docker.list && \
    apt-get update && \
    apt-get install -y docker-ce-cli docker-compose-plugin && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# ------------------------------------------------------------------------
# 🧩 PASO 4: CONFIGURAR EL GRUPO 'docker'
#     → Asegura que el usuario 'jenkins' pueda acceder al socket del host.
# ------------------------------------------------------------------------
# Usa el mismo GID que tiene el grupo 'docker' en el host (normalmente 999, ajusta si difiere)
RUN groupadd -for -g 999 docker && usermod -aG docker jenkins

# ------------------------------------------------------------------------
# 🧩 PASO 5: INSTALAR GOOGLE CHROME HEADLESS (para tests E2E o Angular)
# ------------------------------------------------------------------------
RUN wget -q -O /usr/share/keyrings/google-linux-signing-key.gpg https://dl.google.com/linux/linux_signing_key.pub && \
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/google-linux-signing-key.gpg] http://dl.google.com/linux/chrome/deb/ stable main" \
    > /etc/apt/sources.list.d/google-chrome.list && \
    apt-get update && \
    apt-get install -y google-chrome-stable --no-install-recommends && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# ------------------------------------------------------------------------
# 🧩 PASO 6: VERIFICACIONES OPCIONALES
# ------------------------------------------------------------------------
RUN node -v && npm -v && docker -v && google-chrome --version

# ------------------------------------------------------------------------
# 🧩 PASO 7: VOLVER AL USUARIO 'jenkins'
# ------------------------------------------------------------------------
USER jenkins








# # Partimos de la imagen oficial de Jenkins LTS con JDK 21
# FROM jenkins/jenkins:lts-jdk21

# # Cambiamos a usuario root para poder instalar cosas
# USER root

# ## PASO 1: INSTALAR DEPENDENCIAS BÁSICAS (curl, gnupg2, etc.)
# RUN apt-get update && \
#     # ELIMINADO: software-properties-common
#     apt-get install -y curl gnupg2 lsb-release ca-certificates apt-transport-https && \
#     # Limpiamos las listas de apt después del update/install para reducir el tamaño
#     rm -rf /var/lib/apt/lists/*

# ## PASO 2: INSTALAR NODE.JS Y NPM
# RUN apt-get update && \
#     curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
#     apt-get install -y nodejs && \
#     npm install -g npm@latest && \
#     apt-get clean && rm -rf /var/lib/apt/lists/*

# ## PASO 3: INSTALAR DOCKER CLI (Docker-in-Docker)
# RUN curl -fsSL https://get.docker.com | bash

# # Crear grupo docker (mismo GID que el host)
# RUN groupadd -for -g 999 docker && usermod -aG docker jenkins

# ## ✅ PASO 4: INSTALAR GOOGLE CHROME HEADLESS EN DEBIAN TRIXIE
# RUN apt-get update && \
#     apt-get install -y wget gnupg2 && \
#     wget -q -O /usr/share/keyrings/google-linux-signing-key.gpg https://dl.google.com/linux/linux_signing_key.pub && \
#     echo "deb [arch=amd64 signed-by=/usr/share/keyrings/google-linux-signing-key.gpg] http://dl.google.com/linux/chrome/deb/ stable main" \
#     > /etc/apt/sources.list.d/google-chrome.list && \
#     apt-get update && \
#     apt-get install -y google-chrome-stable --no-install-recommends && \
#     apt-get clean && rm -rf /var/lib/apt/lists/*

# # Verificación opcional (la mantendremos)
# RUN node -v && npm -v && docker -v

# # Regresamos al usuario Jenkins (seguridad)
# USER jenkins