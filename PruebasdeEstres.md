# 📊 Reporte de Pruebas de Carga con Siege

## 📝 Introducción

En este reporte se documentan las pruebas de carga realizadas con la herramienta **Siege** sobre un entorno local. El objetivo fue evaluar el comportamiento de la aplicación ante distintas cantidades de usuarios concurrentes, duraciones y patrones de acceso.

Las pruebas se hicieron antes y después de aplicar mejoras, para observar posibles diferencias en rendimiento. A lo largo del reporte se analizan métricas clave como transacciones, tiempos de respuesta, throughput y tasa de errores.

---

## ⚙️ Herramienta Utilizada

- **Siege**: Utilizada para realizar pruebas de carga HTTP.
- **Entorno**: `localhost:8080` (aplicación corriendo en contenedor Docker)
- **Sistema Operativo**: Parrot OS

---

## 🚀 Pruebas Realizadas

### 1. Prueba básica rápida

```bash
siege -c 5 -r 3 http://localhost:8080
