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


3. Prueba de usuarios limitados:

siege -c 5 -t 30s http://localhost:8080
Se simularon 5 usuarios durante 30 segundos. Es una prueba liviana para verificar el rendimiento a corto plazo con pocos usuarios.
•	Transacciones: 2788
•	Tiempo de respuesta promedio: 0.05 s (excelente)
•	Throughput: 0.33 MB/s

Conclusión:El sistema respondió de forma muy eficiente. Ideal para endpoints livianos o pruebas rápidas de validación.


4. Prueba sin delay:

siege -b -c 5 -t 45s http://localhost:8080
Prueba en modo benchmark, es decir, sin esperar entre peticiones. Los usuarios atacan el servidor a máxima velocidad durante 45 segundos.
Resultados:
•	Transacciones: 3148
•	Tiempo de respuesta promedio: 0.07 s
•	Throughput: 0.25 MB/s

Conclusión: Excelente rendimiento en un escenario de presión máxima. No hubo errores, lo cual muestra buena optimización del servidor.
```
![1](https://github.com/user-attachments/assets/ad583ff4-e4c8-4c87-a725-99375784ea1b)

```
```
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

2. Simulación de usuario real:

siege -i -c 5 -t 1m http://localhost:8080
Prueba en modo aleatorio (-i), útil para simular navegación real. Se usaron 5 usuarios durante 1 minuto.
Resultados:
•	Transacciones: 6540
•	Tiempo de respuesta: 0.04 s
•	Throughput: 0.39 MB/s

Conclusión: Excelente rendimiento en pruebas más realistas. Altísima cantidad de transacciones por segundo (110).


3. Prueba con retardo mínimo:

siege -c 7 -d 1 -t 45s http://localhost:8080
7 usuarios con un delay de 1 segundo entre peticiones. Útil para simular carga moderada pero más realista.
Resultados:
•	Transacciones: 1680
•	Tiempo de respuesta: 0.05 s
•	Throughput: 0.13 MB/s

Conclusión: Buen comportamiento ante una carga realista con pausas. Sin errores, alta disponibilidad.

```
![2](https://github.com/user-attachments/assets/9ea1f9a1-bab8-4731-93a7-d9e8bdfc1374)
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
## Conclusión:

Después de realizar las pruebas de carga con Siege, pude comprobar que la aplicación responde de forma estable incluso bajo diferentes niveles de demanda. En todos los escenarios, los tiempos de respuesta se mantuvieron bajos y no hubo errores ni caídas del servidor, lo cual indica un buen rendimiento general.



