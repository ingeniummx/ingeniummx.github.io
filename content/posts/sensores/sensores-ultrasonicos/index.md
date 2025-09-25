---
title: "Sensores ultrasónicos"
date: 2024-03-16T15:00:00+02:00
description: "Principios de los sensores ultrasónicos de distancia, consideraciones de diseño y estrategias de filtrado."
menu:
  sidebar:
    name: Sensores ultrasónicos
    identifier: sensores-ultrasonicos
    parent: sensores
    weight: 3
hero: images/sensores-ultrasonicos-cover.jpg
tags:
- Ultrasónico
- Distancia
- Robótica móvil
- Sensores
categories:
- Sensores
- Robótica
math: true
---

Los **sensores ultrasónicos** miden distancia calculando el tiempo que tarda un pulso acústico en viajar y regresar desde un obstáculo. Son económicos y útiles en robótica móvil, medición de nivel y domótica.

---

## Componentes clave

- **Transductor emisor/receptor** piezoeléctrico (~40 kHz).
- **Driver** que genera burst de alta tensión (80–120 Vpp en modelos industriales).
- **Detector de eco** con amplificación logarítmica.
- **Controlador** que mide el tiempo de vuelo (ToF) y entrega una lectura digital o analógica.

La distancia se calcula como:

\[
d = \frac{v_{sonido} \cdot t_{eco}}{2}
\]

donde \(v_{sonido}\) ≈ 343 m/s a 20 °C.

---

## Módulos populares

| Modelo | Interfaz | Rango típico | Precisión | Notas |
| --- | --- | --- | --- | --- |
| HC-SR04 | Trigger/Echo (5 V) | 2–400 cm | ±3 mm | Necesita conversión de nivel para 3,3 V. |
| JSN-SR04T | Trigger/Echo, impermeable | 25–600 cm | ±20 mm | Sensor remoto IP66, ideal exterior. |
| MaxBotix MB1240 | PWM, analógica, serie | 20–765 cm | ±2,5 cm | Auto-calibración y filtrado de ruido. |
| Ultrasónicos industriales | 4–20 mA, IO-Link | >8 m | ±1 % | Resisten polvo, alta tensión de excitación. |

---

## Consideraciones de diseño

1. **Ángulo de haz:** típicamente 15°. Objetos pequeños o inclinados pueden no reflejar suficiente energía.
2. **Temperatura y humedad:** afectan la velocidad del sonido. Compensa con sensores de temperatura o tablas.
3. **Ruido eléctrico:** usa capacitores de desacoplo y layout corto entre driver y transductor.
4. **Interferencias cruzadas:** programa ventanas de escucha y IDs si múltiples sensores operan simultáneamente.

---

## Filtrado y procesamiento

- **Promedios móviles** para suavizar lecturas.
- **Filtros mediana** para eliminar ecos falsos.
- **Ventanas de aceptación** basadas en velocidad máxima esperada para descartar saltos.
- **Fusión sensorial** con encoders, IMU o lidar para mejorar precisión.

---

## Aplicaciones

- Evitar colisiones en robots y drones terrestres.
- Medir nivel en tanques de agua o granos.
- Detectar presencia en puertas automáticas.
- Controlar estaciones de estacionamiento y basureros inteligentes.

Seleccionar el módulo adecuado y diseñar el entorno eléctrico y mecánico correctamente garantizará lecturas estables en tus proyectos ultrasónicos.
