# ☁️ Despliegue de UbiSIG en la nube (AWS EC2)

Este documento describe el proceso para instalar y configurar PostgreSQL/PostGIS y GeoServer en una instancia EC2 de AWS (Ubuntu 22.04).

---

## 1. Requisitos previos

- Instancia EC2 en ejecución (t2.medium o superior recomendada)
- Acceso SSH configurado
- DNS dinámico (ej: No-IP) si no usas IP elástica
- Docker y Docker Compose instalados (opcional)

---

## 2. Instalación de PostgreSQL + PostGIS

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib postgis -y
```
Crear base de datos y habilitar PostGIS

```bash
sudo -u postgres psql
CREATE DATABASE ubisig;
\c ubisig
CREATE EXTENSION postgis;
```
---

## 3. Instalación de GeoServer

### 3.1. Instalación de Docker y Docker Compose
```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release

sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \
| sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
Para verificar que Docker funciona correctamente:
```bash
sudo docker run hello-world
```
### 3.2. Añadir tu usuario al grupo docker (opcional)
Para usar Docker sin sudo:
```bash
sudo usermod -aG docker $USER
newgrp docker

### 3.3. Instalar Docker Compose (si no viene instalado por defecto)
```bash
DOCKER_COMPOSE_VERSION=2.24.0
sudo curl -L "https://github.com/docker/compose/releases/download/v$DOCKER_COMPOSE_VERSION/docker-compose-$(uname -s)-$(uname -m)" \
  -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```
### 4. Estado de los servicios
Verificar que Docker está activo:
```bash
systemctl status docker
```

### 5. Instalación de GeoServer. 
```bash
docker run -d --name geoserver \
  -p 8080:8080 \
  -e GEOSERVER_ADMIN_USER=admin \
  -e GEOSERVER_ADMIN_PASSWORD=admin \
  -v $HOME/geoserver_data:/opt/geoserver/data_dir \
  kartoza/geoserver
```
---

## 4. Acceso desde Internet
- Abrir puerto 8080 en el grupo de seguridad de EC2
- Configurar HTTPS con NGNIX y Let's Encrypt (ver docs/ssl_config.md)
- Asociar DNS dinámico o IP elástica
