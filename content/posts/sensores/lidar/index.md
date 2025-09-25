---
title: "Lidar"
date: 2024-03-16T15:30:00+02:00
description: "Conceptos fundamentales del Lidar, tecnologías de escaneo y criterios para elegir un modelo para robótica."
menu:
  sidebar:
    name: Lidar
    identifier: lidar
    parent: sensores
    weight: 4
hero: images/lidar-cover.jpg
tags:
- Lidar
- Robótica
- SLAM
- Sensores de distancia
categories:
- Sensores
- Robótica
---

Los sistemas **Lidar (Light Detection and Ranging)** miden distancias con láser y generan mapas de alta resolución. Son esenciales en SLAM, vehículos autónomos e inspección industrial.

---

## Tecnologías principales

1. **Lidar rotativo 2D:** un láser gira 360° en un plano horizontal. Ejemplos: RPLidar A2, Hokuyo UST.
2. **Lidar rotativo 3D:** varias líneas láser (Velodyne, Ouster) o inclinación mecánica para generar nubes 3D.
3. **Solid-state (MEMS, flash, FMCW):** sin partes móviles, menor tamaño y costo creciente en automoción.
4. **Time-of-Flight directo vs FMCW:** ToF mide tiempo, FMCW mide frecuencia y obtiene velocidad relativa.

---

## Parámetros clave

- **Alcance:** distancia máxima efectiva para objetos con reflectividad estándar.
- **Frecuencia de escaneo:** Hz o RPM; determina cuántos datos por segundo.
- **Resolución angular:** separación entre mediciones; influye en densidad de nube.
- **Campo de visión (FoV):** horizontal y vertical.
- **Precisión y repetibilidad:** error absoluto y ruido entre mediciones.
- **Clase láser:** seguridad ocular (Clase 1 preferida para interiores).

---

## Comparativa rápida

| Modelo | Tipo | Alcance | Frecuencia | Nota destacada |
| --- | --- | --- | --- | --- |
| RPLidar A1 | 2D | 12 m | 5–10 Hz | Económico para hobby, sin protección polvo. |
| Slamtec S1 | 2D | 40 m | 10–20 Hz | IP65, lidar doble motor. |
| Hokuyo UST-10LX | 2D | 10 m | 40 Hz | Industrial, interfaz Ethernet. |
| Ouster OS1 | 3D solid-state | 120 m | 10–20 Hz | Alta densidad, APIs ROS listas. |
| Livox MID-360 | 3D no rotativo | 70 m | 20 Hz | Escaneo no repetitivo con fusión temporal. |

---

## Integración en robots

1. **Montaje rígido** y calibración de transformaciones TF para ROS/ROS 2.
2. **Sincronización temporal** con IMU o cámaras para SLAM preciso.
3. **Filtrado de ruido** (VoxelGrid, StatisticalOutlierRemoval) antes de alimentar algoritmos.
4. **Gestión de energía**: algunos modelos consumen >10 W, planifica la fuente.

---

## Buenas prácticas

- Mantén **superficies limpias**; polvo o insectos generan falsos positivos.
- Usa **zonas de exclusión** en software para ignorar partes del robot.
- Configura **firmware y drivers oficiales** para aprovechar actualizaciones de calibración.
- Considera **climatización** si operas en exterior (calentadores, deshumidificadores).

Con estos criterios podrás elegir e integrar un Lidar acorde a tus necesidades de navegación y percepción.
