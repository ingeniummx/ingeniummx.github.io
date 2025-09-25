---
title: "Instalar micro-ROS en la Pico"
date: 2024-03-16T18:30:00+02:00
description: "Procedimiento para integrar micro-ROS en la Raspberry Pi Pico usando el Pico SDK y colcon."
menu:
  sidebar:
    name: Instalar micro-ROS en la Pico
    identifier: instalar-micro-ros-pico
    parent: microcontroladores
    weight: 6
hero: images/instalar-micro-ros-pico-cover.jpg
tags:
- micro-ROS
- Raspberry Pi Pico
- ROS 2
- Embedded
categories:
- Microcontroladores
- Robótica
---

**micro-ROS** lleva ROS 2 a microcontroladores con recursos limitados. A continuación se describe cómo configurar el build system para la Raspberry Pi Pico utilizando el Pico SDK oficial.

---

## Prerrequisitos

- Toolchain Arm (`arm-none-eabi-gcc`), CMake y Ninja.
- ROS 2 Humble (o posterior) instalado en tu sistema.
- Repositorio `micro_ros_setup` clonado en el workspace de ROS 2.
- Pico SDK inicializado (`PICO_SDK_PATH`).

---

## Configurar el workspace

```bash
source /opt/ros/humble/setup.bash
mkdir -p ~/micro_ros_ws/src
cd ~/micro_ros_ws/src
git clone https://github.com/micro-ROS/micro_ros_setup.git
cd ..
colcon build
source install/local_setup.bash
```

---

## Crear firmware para la Pico

1. **Importa los repositorios necesarios**:

```bash
ros2 run micro_ros_setup create_firmware_ws.sh freertos raspberripi_pico
```

2. **Descarga dependencias**:

```bash
ros2 run micro_ros_setup configure_firmware.sh freertos raspberripi_pico
```

3. **Compila** con opciones personalizadas (Wi-Fi deshabilitada por defecto):

```bash
ros2 run micro_ros_setup build_firmware.sh
```

4. El resultado (`firmware.uf2`) se genera en `firmware/build`. Cópialo a la Pico en modo BOOTSEL.

---

## Ejemplo: nodo ping

Tras flashear, conecta la Pico por USB y ejecuta en el host:

```bash
ros2 run micro_ros_agent micro_ros_agent serial --dev /dev/ttyACM0 -b 115200
```

En otra terminal:

```bash
ros2 topic echo /micro_ros/ping
```

Deberías ver paquetes periódicos generados por el firmware de ejemplo.

---

## Personalizar el proyecto

- Edita `firmware/freertos_apps/apps/ping_pong/main.c` para crear tus propios nodos.
- Ajusta memoria en `pico_hw_description.cmake` según tus necesidades de pila y heap.
- Integra sensores usando `rclc` y colas de mensajes confiables (`rmw_microxrcedds`).

---

## Buenas prácticas

1. Actualiza periódicamente `micro_ros_setup` para obtener parches de compatibilidad.
2. Usa **colcon profiles** para separar builds de depuración y liberación.
3. Registra el hash del commit del Pico SDK y micro-ROS en tu documentación.
4. Automatiza generación de `.uf2` en CI para mantener firmware consistente.

Siguiendo estos pasos ejecutarás micro-ROS en la Raspberry Pi Pico con el SDK oficial listo para integrarse en sistemas ROS 2.
