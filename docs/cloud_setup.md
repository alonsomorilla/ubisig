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
