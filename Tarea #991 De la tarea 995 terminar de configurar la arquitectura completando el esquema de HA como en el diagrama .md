#  Reporte Final de Pruebas - Arquitectura en Alta Disponibilidad (HA)

##  Objetivo

El propósito de este reporte es documentar y justificar las pruebas realizadas una vez finalizada la configuración completa de la arquitectura de Alta Disponibilidad (HA) para WordPress y MariaDB. Esto incluye la incorporación de múltiples nodos y el uso de HAProxy como balanceador de carga, cumpliendo así el diseño planteado inicialmente.

---

## 🛠️ Arquitectura Final

La arquitectura final está compuesta por:

- **WordPress en HA** con **3 nodos**.
- **MariaDB en HA** con **3 nodos**.
- **2 instancias de HAProxy**, configuradas para redirigir tráfico a los servicios en los puertos 80 (nodo master) y 8080 (nodo slave).

Esta configuración asegura balanceo de carga, redundancia y alta disponibilidad para todos los servicios involucrados.

---

## 🔍 Pruebas Realizadas

Se realizaron 3 pruebas para evaluar el rendimiento y estabilidad de la arquitectura:

### 📷 Prueba 1: ApacheBench sobre puerto 80 (nodo master)

- Tiempo total: **18.538 s**
- Requests por segundo: **13.49 req/s**
- Tiempo medio por request: **74.15 ms**
- Latencia máxima: **3848 ms**

### 📷 Prueba 2: ApacheBench sobre puerto 8080 (nodo slave)

- Tiempo total: **7.794 s**
- Requests por segundo: **32.07 req/s**
- Tiempo medio por request: **249.424 ms**
- Latencia máxima: **769 ms**

### 📷 Prueba 3: Siege sobre ambos nodos

**Nodo master (puerto 80):**
- Transacciones: **970**
- Tiempo de respuesta: **0.54 s**
- Tasa de transacción: **32.67 trans/s**

**Nodo slave (puerto 8080):**
- Transacciones: **1015**
- Tiempo de respuesta: **0.54 s**
- Tasa de transacción: **32.68 trans/s**

---

## ✅ Justificación

La ejecución de estas pruebas valida que la arquitectura cumple con los principios de alta disponibilidad y balanceo de carga. Al comparar los resultados entre los nodos master y slave:

- Se observa un mejor rendimiento del nodo slave en términos de latencia y procesamiento.
- Ambos nodos alcanzan una tasa de transacción similar cuando se prueba con Siege, demostrando eficiencia y equilibrio.
- El uso de dos HAProxy permite distribuir pruebas (AB y Siege) entre los nodos sin interferencias, reflejando un sistema robusto y escalable.

---

## 🧾 Conclusión

La implementación final cumple con el diagrama de arquitectura propuesto, garantizando alta disponibilidad mediante la réplica de nodos y balanceo de carga efectivo. Las pruebas demuestran que la arquitectura es capaz de manejar cargas concurrentes distribuidas con buena estabilidad y rendimiento.

Este entorno está listo para operaciones en producción con tolerancia a fallos y capacidad de escalar según las necesidades del servicio.
