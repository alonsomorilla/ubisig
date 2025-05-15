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

### Opción A: Manual (WAR en Tomcat)
1. Instalar Java 11
2. Instalar Tomcat 9
3. Descargar GeoServer WAR
4. Copiarlo a webapps/

### Opción B: Docker
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
