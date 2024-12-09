# Usa la imagen base oficial del agente Jenkins
FROM jenkins/inbound-agent:latest

# Cambiar al usuario root para instalar las herramientas
USER root

# Instalar Docker CLI
RUN apt-get update && \
    apt-get install -y docker.io && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Instalar Node.js (última versión LTS)
RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g npm@latest && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Instalar kubectl
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && \
    install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl && \
    rm kubectl

# Configurar permisos para Docker
RUN groupadd -g $(stat -c '%g' /var/run/docker.sock) systemd-journal || true && \
    usermod -aG systemd-journal jenkins

# Cambiar de nuevo al usuario Jenkins
USER jenkins

# Configurar el directorio de trabajo
WORKDIR /home/jenkins/agent