# Simulación del Problema de Tres Cuerpos

## Resumen
Se implementó una simulación numérica del problema de tres cuerpos usando Python y Matplotlib para modelar las interacciones gravitacionales entre el Sol, la Tierra y Venus. El código resuelve las ecuaciones de movimiento mediante un integrador explícito (actualización de velocidades y luego posiciones — Euler semi-implícito), registra trayectorias y genera una animación interactiva con control de velocidad en tiempo real.  
Los resultados muestran órbitas estables y la relación esperada entre distancias orbitales y periodos (Venus completa más órbitas que la Tierra). Se discuten limitaciones numéricas y mejoras propuestas (RK4, suavizado, control de energía).

## 1. Introducción
El problema de los tres cuerpos es un problema clásico de la mecánica celeste: dado tres masas que interactúan únicamente por gravedad newtoniana, predecir sus trayectorias. A diferencia del caso de dos cuerpos, no existe una solución analítica general y las soluciones pueden presentar comportamiento caótico.

El objetivo de esta práctica fue implementar una simulación numérica simple que permita:
- Visualizar las trayectorias en 2D.
- Permitir control en tiempo real de la velocidad de simulación.
- Analizar la concordancia con predicciones teóricas básicas (leyes de Kepler y comportamiento orbital).

## 2. Metodología

### 2.1 Ecuaciones físicas
Se utiliza la ley de gravitación de Newton y la segunda ley de Newton. Para cada cuerpo *i* la aceleración viene de:

\[
ec{a}_i = G \sum_{j 
eq i} rac{m_j}{\|ec{r}_j - ec{r}_i\|^3} (ec{r}_j - ec{r}_i)
\]

donde  
\(
G = 6.67430 	imes 10^{-11} \, 	ext{m}^3 \, 	ext{kg}^{-1} \, 	ext{s}^{-2}
\)

### 2.2 Integrador numérico
En cada paso de tiempo \(\Delta t\) (variable según `velocidad_factor`) se realiza:
1. Calcular aceleraciones \(ec{a}_i\) por la suma de pares.
2. Actualizar velocidades: \(ec{v}_i \leftarrow ec{v}_i + ec{a}_i \, \Delta t\).
3. Actualizar posiciones: \(ec{r}_i \leftarrow ec{r}_i + ec{v}_i \, \Delta t\).

Este esquema (velocities then positions) es el **Euler semi-implícito** (Euler-Cromer), que es más estable para sistemas oscilatorios que el Euler explícito simple.

### 2.3 Condiciones iniciales
Valores reales aproximados:
- **Sol**: \(m = 1.989 	imes 10^{30} \, 	ext{kg}\), posición \((0,0)\).
- **Tierra**: \(m = 5.972 	imes 10^{24} \, 	ext{kg}\), posición \((1.496	imes 10^{11}, 0)\) m, velocidad \((0,29780)\) m/s.
- **Venus**: \(m = 4.867 	imes 10^{24} \, 	ext{kg}\), posición \((1.082	imes 10^{11}, 0)\) m, velocidad \((0,35020)\) m/s.

Paso de tiempo base: \(\Delta t_	ext{base} = 3600\) s (1 h). Control de velocidad con teclas ↑ y ↓ (dobla / divide por 2, máximo x64).

### 2.4 Implementación
- **Lenguaje**: Python 3.x  
- **Librerías**: numpy, matplotlib (FuncAnimation)  
- Física en función `update()` y almacenamiento de trayectorias para graficar.

## 3. Resultados y observaciones
- Órbitas casi circulares de Tierra (azul) y Venus (naranja) alrededor del Sol (amarillo).
- Venus completa más órbitas en el mismo tiempo que la Tierra.
- Movimiento pequeño del Sol por efecto del centro de masas.
- Estabilidad general sin colisiones.
- Precesión leve en Venus por interacción con la Tierra.
- A mayor velocidad_factor los días pasan más rápido, consistente con la escala temporal.

## 4. Comparación con la teoría
- **Ley de Kepler**: \(T^2 \propto a^3\) se cumple cualitativamente.
- **Velocidad orbital**: coherente con \(v pprox \sqrt{GM_\odot/r}\).
- **Conservación del momento lineal**: centro de masas casi constante.

## 5. Limitaciones
- Euler-Cromer acumula error energético a largo plazo.
- Δt demasiado grande genera errores.
- No hay modelado de colisiones ni suavizado.
- No se validó energía ni periodos con valores teóricos.

## 6. Mejoras futuras
- Implementar RK4 o integrador symplectic.
- Añadir softening para evitar singularidades.
- Graficar energía total y momento.
- Añadir interfaz gráfica con sliders.
- Ampliar el sistema con más planetas.

## 7. Conclusiones
La simulación reproduce de forma cualitativa el sistema Sol–Tierra–Venus con resultados coherentes y visualmente claros. Es útil como herramienta educativa pero requiere mejoras numéricas para análisis precisos.

## Referencias
- Newton, I. *Philosophiæ Naturalis Principia Mathematica*.
- Murray C.D., Dermott S.F., *Solar System Dynamics*. Cambridge Univ. Press.
- Datos físicos: tablas astronómicas públicas.
