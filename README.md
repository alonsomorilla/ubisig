# UbiSIG: Infraestructura geoespacial automatizada y portable

## Fase 1: Infraestructura mínima en AWS

### Objetivo
Diseñar y desplegar una arquitectura base en AWS como entorno para servidores geoespaciales como GeoServer.

### Componentes de la arquitectura

- VPC con subred pública
- EC2 (Ubuntu) con IP elástica
- Security Group personalizado
- IAM Role con acceso controlado a S3
- S3 bucket para almacenamiento geográfico
- (Opcional) Route 53 o DNS dinámico

### Diagrama de arquitectura
![Diagrama](enlace_o_captura_luego)

### Pasos realizados

1. Crear la VPC y subred
2. Lanzar instancia EC2 con Ubuntu
3. Asociar IP elástica
4. Configurar Security Group (puertos: 22, 8080, 80)
5. Crear IAM Role y asociarlo a EC2
6. Crear bucket S3 para backup o capas
7. Documentar configuraciones y comandos en este repo

### Próximos pasos

- Automatizar instalación de GeoServer con Ansible
- Desplegar infraestructura con Terraform
- Integrar almacenamiento SIG desde S3

### Créditos

Este proyecto forma parte de mi formación como arquitecto cloud y especialista en SIG. Más información: [https://www.linkedin.com/in/almorillam/].

