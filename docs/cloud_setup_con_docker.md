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
```

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

### 🔓 Reglas de acceso recomendadas (puertos en grupo de seguridad de AWS)

Para que el sistema sea accesible desde fuera, asegúrate de que el grupo de seguridad de tu instancia EC2 permita los siguientes puertos:

| Servicio          | Puerto | Protocolo | Origen recomendado |
|-------------------|--------|-----------|---------------------|
| GeoServer (HTTP)  | 8080   | TCP       | Solo IPs autorizadas o 0.0.0.0/0 para acceso abierto |
| PostgreSQL/PostGIS| 5432   | TCP       | Solo IPs autorizadas (ej: IP de tu oficina o servidor) |
| SSH               | 22     | TCP       | Solo tu IP personal (por seguridad) |

> ⚠️ **Importante:** nunca abras el puerto 5432 a `0.0.0.0/0` sin autenticar ni proteger tu base de datos. Limítalo a rangos específicos o redes privadas siempre que sea posible.

### 🛡️ Consejo de seguridad adicional
Si GeoServer está en el mismo host (o en el mismo docker-compose), y no necesitas acceso externo a PostgreSQL, puedes:

No abrir el puerto 5432 en el grupo de seguridad.

Conectar por localhost o red interna Docker y evitar exposición externa.
