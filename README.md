# UbiSIG: Infraestructura geoespacial automatizada y portable

# 🌍 UbiSIG — Sistema geoespacial portátil y sincronizado

**UbiSIG** es un sistema geoespacial diseñado para funcionar tanto offline como online. Ideal para trabajos de campo en zonas sin cobertura, permite que equipos técnicos o comunidades locales recojan, almacenen y sincronicen datos espaciales desde una Raspberry Pi a un servidor en la nube.

Este proyecto está en desarrollo, pero su arquitectura modular y de bajo coste lo hace escalable, ético y replicable.

---

## 🚀 Casos de uso reales

1. **Inventario de flora sin cobertura:** Equipos de biólogos usan tablets conectadas a la Raspberry Pi para registrar especies en campo, sincronizando después con la base de datos central.
2. **Prevención de incendios:** Brigadas forestales documentan trabajos y riesgos georreferenciados sin necesidad de conexión.
3. **Diagnóstico rural colaborativo:** Personas locales y técnicos recogen datos sobre caminos, servicios, acceso a recursos, etc., con posibilidad de visualización y análisis posterior.

---

## 🛠 Tecnologías utilizadas

- **Raspberry Pi 5** con Raspbian OS
- **Docker + Docker Compose**
- **GeoServer + PostGIS**
- **Leaflet + GeoJSON**
- **Rsync / API REST** para sincronización de datos
- **No-IP o DYNU**, para DNS dinámico
- **Router 4G** para conectividad portátil

---

## 🧰 Estructura del proyecto (Documentación detallada en la carpeta /docs)

```bash
ubisig/
├── README.md
├── LICENSE
├── .gitignore
├── docs/
│   ├── arquitectura.md
│   ├── raspberry_setup.md
│   ├── cloud_setup.md
│   └── casos_uso.md
├── scripts/
│   ├── raspberry/
│   │   ├── instalar_geoserver.sh
│   │   └── montar_intranet_wifi.sh
│   └── cloud/
│       ├── configurar_ssl.sh
│       └── backup_postgis.sh
└── deploy/
    ├── docker-compose.yml
    └── nginx.conf

---

## 🛠️ Cómo empezar

### 1. Clonar el repositorio

```bash
git clone https://github.com/alonsomorilla/ubisig.git
cd ubisig
```

### 2. Configurar entorno

- [Instrucciones para Raspberry Pi](docs/raspberry_setup.md)
- [Despliegue del servidor en la nube](docs/cloud_setup.md)

---

## 📚 Documentación adicional

- [Arquitectura del sistema](docs/arquitectura.md)
- [Descripción ampliada de los casos de uso](docs/casos_uso.md)

---

## 📝 Licencia

Este proyecto se distribuye bajo licencia **AGPL-3.0**. Consulta el archivo [LICENSE](LICENSE).

---

## 🤝 ¿Quieres colaborar?

Propon ideas, informa de errores o contacta conmigo por [LinkedIn](https://www.linkedin.com/in/alonsomorilla).
