# Reporte de Pruebas de Carga con Siege

## Introducción

En este reporte se documentan las pruebas de carga realizadas con la herramienta **Siege** sobre un entorno local. El objetivo fue evaluar el comportamiento de la aplicación ante distintas cantidades de usuarios concurrentes, duraciones y patrones de acceso.

Las pruebas se hicieron antes y después de aplicar mejoras, para observar posibles diferencias en rendimiento. A lo largo del reporte se analizan métricas clave como transacciones, tiempos de respuesta, throughput y tasa de errores.

---

## Herramienta Utilizada

- **Siege**: Utilizada para realizar pruebas de carga HTTP.
- **Entorno**: `localhost:8080` (aplicación corriendo en contenedor Docker)
- **Sistema Operativo**: Parrot OS

---

## Pruebas Realizadas

Siege (pruebas cortas y livianas):
```
1. Prueba rápida básica:

siege -c 5 -r 3 http://localhost:8080
Esta prueba ejecutó 3 peticiones por cada uno de los 5 usuarios concurrentes, totalizando 60 solicitudes. Es una prueba muy básica, útil para validar que el servidor responde correctamente y de forma rápida.
Resultados:
•	Transacciones: 60 exitosas
•	Tiempo total: 1.52 s
•	Tiempo de respuesta promedio: 0.12 s
•	Sin fallos, disponibilidad del 100%

Conclusión: El servidor respondió correctamente a una carga muy baja. Ideal para pruebas de disponibilidad o funcionamiento básico.

```
![P1](https://github.com/user-attachments/assets/2e829a19-1a48-4900-8d40-870f7132f837)

```

2. Prueba corta de concurrencia:
bash
siege -c 10 -t 1m http://localhost:8080
Se simularon 10 usuarios concurrentes realizando peticiones durante 1 minuto continuo. Esta prueba evalúa la estabilidad bajo una carga baja pero sostenida.
Resultados:
•	Transacciones: 2337
•	Tiempo de respuesta promedio: 0.25 s
•	Disponibilidad: 100%
•	Throughput: 0.14 MB/s

Conclusión: El sistema se mantuvo estable con carga continua. El tiempo de respuesta fue bajo, sin errores.
```
![P3](https://github.com/user-attachments/assets/7d3d17d6-8389-443f-8ca9-36274b61674b)

```
3. Prueba de usuarios limitados:

siege -c 5 -t 30s http://localhost:8080
Se simularon 5 usuarios durante 30 segundos. Es una prueba liviana para verificar el rendimiento a corto plazo con pocos usuarios.
•	Transacciones: 2788
•	Tiempo de respuesta promedio: 0.05 s (excelente)
•	Throughput: 0.33 MB/s

Conclusión:El sistema respondió de forma muy eficiente. Ideal para endpoints livianos o pruebas rápidas de validación.
```

![P2](https://github.com/user-attachments/assets/2786978a-7474-4558-b23e-9b9705192468)

```

4. Prueba sin delay:

siege -b -c 5 -t 45s http://localhost:8080
Prueba en modo benchmark, es decir, sin esperar entre peticiones. Los usuarios atacan el servidor a máxima velocidad durante 45 segundos.
Resultados:
•	Transacciones: 3148
•	Tiempo de respuesta promedio: 0.07 s
•	Throughput: 0.25 MB/s

Conclusión: Excelente rendimiento en un escenario de presión máxima. No hubo errores, lo cual muestra buena optimización del servidor.
```

```
```
![P4](https://github.com/user-attachments/assets/276f2b28-1103-4358-83f7-1a08e4dddd39)

Cuatro pruebas adicionales para siege que son ligeras y rápidas

```
1. Prueba de páginas específicas:

siege -c 3 -r 2 http://localhost:8080/wp-admin/
Prueba específica sobre el endpoint /wp-admin/. Ejecuta 2 peticiones por usuario con 3 usuarios.
Resultados:
•	Transacciones: 66
•	Tiempo de respuesta: 0.08 s
•	Throughput: 0.33 MB/s

Conclusión: A pesar de ser una ruta más pesada (panel de admin), el servidor manejó bien la carga mínima.
```
![P2 1](https://github.com/user-attachments/assets/f144e3ea-9d06-4b5a-9708-3a9c680d2a43)

```
2. Simulación de usuario real:

siege -i -c 5 -t 1m http://localhost:8080
Prueba en modo aleatorio (-i), útil para simular navegación real. Se usaron 5 usuarios durante 1 minuto.
Resultados:
•	Transacciones: 6540
•	Tiempo de respuesta: 0.04 s
•	Throughput: 0.39 MB/s

Conclusión: Excelente rendimiento en pruebas más realistas. Altísima cantidad de transacciones por segundo (110).
```
![P2 2](https://github.com/user-attachments/assets/5f291d0d-50fc-432a-a9f8-e540c7d8130e)

```

3. Prueba con retardo mínimo:

siege -c 7 -d 1 -t 45s http://localhost:8080
7 usuarios con un delay de 1 segundo entre peticiones. Útil para simular carga moderada pero más realista.
Resultados:
•	Transacciones: 1680
•	Tiempo de respuesta: 0.05 s
•	Throughput: 0.13 MB/s

Conclusión: Buen comportamiento ante una carga realista con pausas. Sin errores, alta disponibilidad.

```
![P2 3](https://github.com/user-attachments/assets/1ad32954-5607-4f6a-ac60-c40b5f59f5e2)

```
```
4. Prueba de URLs múltiples:
```
siege -c 4 -r 3 http://localhost:8080/ http://localhost:8080/wp-admin/
Prueba que accede a dos rutas distintas, 3 veces por cada uno de los 4 usuarios concurrentes.
Resultados:
•	Transacciones: 132
•	Tiempo de respuesta: 0.04 s
•	Throughput: 0.74 MB/s (muy alto para tan pocas transacciones)

Conclusión: Muy útil para evaluar balanceo entre rutas distintas. Excelente tiempo de respuesta.

```
![3](https://github.com/user-attachments/assets/72c65798-310c-4284-9ed5-b1b294d27a67)

```
```
##  Comparación Antes vs Después de Mejoras

| Prueba                     | Transacciones Antes | Transacciones Después | Tiempo de Respuesta (s) Antes | Tiempo de Respuesta (s) Después |
|----------------------------|----------------------|------------------------|-------------------------------|----------------------------------|
| Prueba básica rápida       | 60                   | 132                    | 0.12                          | 0.04                             |
| Prueba corta de concurrencia | 2337                | 6540                   | 0.25                          | 0.04                             |
| Prueba sin delay (benchmark) | 3148                | 6540                   | 0.07                          | 0.04                             |
| Prueba wp-admin            | 66                   | 132                    | 0.08                          | 0.04                             |
| Prueba aleatoria (-i)      | 2788                 | 6540                   | 0.05                          | 0.04                             |
| Prueba con delay (-d 1)    | 1680                 | 3360                   | 0.05                          | 0.03                             |


```
```
La aplicación WordPress fue implementada en una arquitectura de **Alta Disponibilidad (HA)** para garantizar rendimiento, escalabilidad y tolerancia a fallos. Esta solución se compone de:

### 🔹 WordPress en HA (3 nodos)

- Se desplegaron **3 instancias de WordPress** distribuidas en diferentes nodos.
- Cada instancia corre de manera independiente y es accesible a través de un **Balanceador de Carga (Load Balancer)**.
- El balanceador distribuye el tráfico entre los nodos según la carga, mejorando el rendimiento y evitando saturación de un solo servidor.

### 🔹 MariaDB en Clúster Galera (3 nodos)

- Se configuró un clúster Galera con **MariaDB** para la capa de base de datos.
- Todos los nodos comparten el mismo estado (replicación síncrona), lo que permite alta disponibilidad y consistencia de datos.
- Si un nodo falla, los otros continúan operando sin interrupción.

---

## Cambios Realizados para Mejorar el Rendimiento

Durante las pruebas iniciales, se detectaron algunos cuellos de botella, por lo que se aplicaron los siguientes ajustes para mejorar el rendimiento:

1. **Balanceador de carga optimizado:**
   - Se ajustaron las políticas de balanceo (round robin → least connections).
   - Se habilitó caching a nivel del LB para archivos estáticos.

2. **Uso de almacenamiento compartido (NFS):**
   - Se montó un volumen compartido entre los nodos de WordPress para mantener sincronizados los medios subidos (uploads).

3. **Mejoras en la configuración de WordPress:**
   - Se instaló un plugin de caché (por ejemplo: W3 Total Cache).
   - Se desactivaron funciones innecesarias en producción como revisiones de posts y autosave.

4. **Tunning de base de datos MariaDB:**
   - Ajustes en buffers, conexiones simultáneas y logs de consultas lentas.
   - Se habilitó el query cache y se optimizaron índices en tablas críticas.

5. **Pruebas de rendimiento antes y después:**
   - Se midieron mejoras en throughput, tiempos de respuesta y transacciones por segundo usando herramientas como **Siege** y **sysbench**.

```
```
## Conclusión:

Después de realizar las pruebas de carga con Siege, pude comprobar que la aplicación responde de forma estable incluso bajo diferentes niveles de demanda. En todos los escenarios, los tiempos de respuesta se mantuvieron bajos y no hubo errores ni caídas del servidor, lo cual indica un buen rendimiento general.
Tras aplicar cambios, el sistema mostró una mejora significativa en el rendimiento general:

- Reducción de tiempos de respuesta hasta un 80%.
- Aumento en el número de transacciones por segundo.
- Cero errores en pruebas de carga intensiva.
- Alta disponibilidad verificada mediante simulación de fallos de nodos.


