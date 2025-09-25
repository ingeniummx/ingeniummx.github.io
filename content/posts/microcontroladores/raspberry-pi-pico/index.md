---
title: "Raspberry Pi Pico"
date: 2024-03-16T11:00:00+02:00
description: "Características clave del RP2040, periféricos destacados y usos típicos de la Raspberry Pi Pico."
menu:
  sidebar:
    name: Raspberry Pi Pico
    identifier: raspberry-pi-pico
    parent: microcontroladores
    weight: 1
hero: images/raspberry-pi-pico-cover.jpg
tags:
- Raspberry Pi Pico
- RP2040
- Microcontroladores
- Prototipado
categories:
- Microcontroladores
- Hardware
---

La **Raspberry Pi Pico** es la primera placa oficial basada en el microcontrolador RP2040 de la Fundación Raspberry Pi. Combina precio contenido, doble núcleo Cortex-M0+ y periféricos flexibles que la convierten en una plataforma ideal para proyectos embebidos y de enseñanza.

---

## Especificaciones esenciales

- **Microcontrolador:** RP2040 con dos núcleos Arm Cortex-M0+ a 133 MHz (overclock estable hasta ~250 MHz).
- **Memoria:** 264 KB de SRAM repartida en bancos y hasta 16 MB de flash externa QSPI.
- **Periféricos:** 30 GPIO, 2×USB 1.1, 2×UART, 2×I²C, 2×SPI, 16 canales PWM, ADC de 12 bits, 8 máquinas PIO.
- **Alimentación:** 1,8–5,5 V, regulador buck-boost integrado.
- **Formatos:** Pico estándar, Pico W (Wi-Fi), Pico H (headers soldados) y módulos embebibles.

---

## Fortalezas del RP2040

1. **PIO (Programmable I/O).** Motores de estado que permiten implementar protocolos personalizados a nivel de hardware.
2. **Doble núcleo.** Separar tareas críticas en un core mientras el otro maneja lógica de alto nivel.
3. **Bajo costo y disponibilidad.** La placa base ronda los 4 USD.
4. **Comunidad y documentación amplia.** SDK en C/C++, MicroPython, CircuitPython y ejemplos oficiales.

---

## Casos de uso recomendados

- **Controladores de robots educativos** gracias a su PWM abundante y soporte MicroPython.
- **Interfaces personalizadas** (DVI, VGA, audio PDM) mediante PIO.
- **IoT económico** con el modelo Pico W y su módulo Wi-Fi CYW43439.
- **Instrumentos de laboratorio DIY** como generadores de señales y analizadores lógicos.

---

## Ecosistema de software

- **Pico SDK (C/C++).** Acceso directo al hardware con CMake, soporte para FreeRTOS y drivers oficiales.
- **MicroPython y CircuitPython.** Repl inmediato, ideal para iterar rápido.
- **TinyUSB.** Implementaciones de dispositivos USB (CDC, HID, MIDI) listas para usar.
- **Bibliotecas de terceros.** Drivers para pantallas, sensores y motores mantenidos por la comunidad.

---

## Consejos para proyectos

1. Usa el **SMPS integrado** (VSYS) para alimentar sensores de 3,3 V y evita exceder 300 mA.
2. Aprovecha el **debug por SWD** expuesto en los pads debajo de la placa.
3. Para aplicaciones críticas, añade **flash externa de calidad** y watchdog habilitado.
4. Documenta la versión del SDK en tu repositorio para reproducibilidad.

Con estos puntos tendrás un panorama claro de lo que ofrece la Raspberry Pi Pico y podrás decidir cuándo utilizarla frente a otras opciones.
