---
title: "Herramientas indispensables para ROS 2"
date: 2024-03-16T12:00:00+02:00
description: "Panorama de utilidades como ros2 topic, RViz, PlotJuggler y Foxglove para depurar y visualizar sistemas ROS 2."
menu:
  sidebar:
    name: Herramientas ROS 2
    identifier: herramientas-ros2
    parent: robotica
    weight: 1
hero: images/herramientas-ros2-cover.jpg
tags:
- ROS 2
- Robótica
- Herramientas de depuración
- Visualización de datos
categories:
- Robótica
- Software
---

ROS 2 ofrece múltiples utilidades para monitorear tópicos, analizar logs y depurar sistemas distribuidos. Conocer las herramientas correctas acelera el desarrollo y evita perder horas persiguiendo bugs.

---

## Línea de comandos

- **`ros2 topic`**: inspecciona publicadores, suscriptores y contenido en tiempo real (`ros2 topic echo`, `ros2 topic hz`). Útil para validar tipos de mensajes y latencias.
- **`ros2 node`**: lista nodos activos, interfaces y parámetros expuestos.
- **`ros2 param`**: permite leer/escribir parámetros dinámicos, ideal para ajustar PIDs o constantes sin recompilar.
- **`ros2 bag`**: registra y reproduce mensajes DDS para depuración offline o análisis de regresiones.

---

## Visualización 3D con RViz

RViz es la navaja suiza para visualizar mapas, nubes de puntos y frames TF.

1. Crea **displays** para láser, cámaras, odometría y TF.
2. Usa **Fixed Frame** coherente (por ejemplo `map` o `base_link`).
3. Configura **markers** y **interactive markers** para depurar planes y acciones.
4. Exporta configuraciones en `.rviz` para compartir con tu equipo.

---

## PlotJuggler

Herramienta especializada en series de tiempo.

- Conecta vía plugin ROS 2 para recibir tópicos directamente.
- Crea **dashboards** con filtros y expresiones matemáticas.
- Exporta a CSV o ROS bag para análisis posterior.
- Ideal para ajustar controladores PID, monitorear corrientes y tensiones en robots móviles.

---

## RQt y paneles personalizados

RQt (Qt + plugins ROS) permite construir interfaces gráficas rápidas:

- **rqt_graph** para visualizar la topología de nodos.
- **rqt_console** y **rqt_logger_level** para filtrar logs.
- **rqt_reconfigure** (con `ros2_control` o `dynamic_reconfigure`) para ajustar parámetros en vivo.

---

## Foxglove Studio

Una opción moderna multiplataforma que se conecta mediante Foxglove Bridge o rosbridge.

- Visualiza tópicos 2D/3D, dashboards y mapas en el navegador.
- Graba **Foxglove Data Platform** o reproduce ROS bag.
- Integra anotaciones colaborativas y métricas de rendimiento.

---

## Buenas prácticas

1. **Mantén plantillas de configuración.** Almacena layouts de RViz, PlotJuggler y Foxglove en tu repositorio.
2. **Automatiza capturas.** Usa `ros2 bag` en pipelines CI para comparar ejecuciones.
3. **Unifica namespaces.** Consistencia en nombres simplifica filtros y scripts.
4. **Documenta flujos de trabajo.** Define qué herramienta usar en cada etapa: bring-up, calibración, pruebas en campo.

Dominar estas utilidades te permitirá diagnosticar robots ROS 2 con rapidez y trabajar de forma colaborativa con tu equipo.
