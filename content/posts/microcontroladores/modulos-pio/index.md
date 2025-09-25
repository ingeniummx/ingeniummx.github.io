---
title: "Módulos PIO en la Pico"
date: 2024-03-16T13:00:00+02:00
description: "Cómo funcionan los módulos PIO de la Raspberry Pi Pico y ejemplos prácticos para crear periféricos personalizados."
menu:
  sidebar:
    name: Módulos PIO en la Pico
    identifier: modulos-pio
    parent: microcontroladores
    weight: 3
hero: images/modulos-pio-cover.jpg
tags:
- PIO
- Raspberry Pi Pico
- RP2040
- Protocolos personalizados
categories:
- Microcontroladores
- Hardware
---

Los **módulos PIO (Programmable I/O)** del RP2040 permiten implementar periféricos programables sin cargar la CPU. Con dos bloques PIO y cuatro máquinas de estado cada uno, puedes crear controladores para protocolos no soportados de forma nativa.

---

## Arquitectura PIO

- **Máquinas de estado:** hasta 4 por bloque, cada una con 32 instrucciones de 16 bits.
- **FIFO TX/RX:** buffers de 4 palabras para intercambio con la CPU o DMA.
- **Pins flexibles:** mapeo independiente para entrada, salida y side-set.
- **Clock divider:** ajusta la frecuencia de ejecución con resolución fraccional.

---

## Flujo de trabajo

1. Escribe el programa PIO en ensamblador específico (`.program`).
2. Carga el código en la instrucción memory del bloque PIO.
3. Configura los registros de control (SM, clocks, pins).
4. Usa la API del SDK o MicroPython para enviar/recibir datos.

---

## Ejemplos prácticos

### WS2812 (NeoPixel)

- Genera los pulsos de 800 kHz con side-set para controlar tiras RGB.
- Libera a la CPU para calcular animaciones mientras PIO gestiona la temporización.

### Interfaces VGA/DVI

- Emite señales de sincronía y datos de vídeo usando múltiples máquinas coordinadas.
- Requiere DMA para alimentar los buffers a alta velocidad.

### Captura de señales

- Configura una máquina PIO en modo `IN` para muestrear pines a alta frecuencia.
- Vuelca los datos a memoria mediante DMA para análisis posterior.

---

## Consejos de implementación

1. **Planifica el ancho de palabra.** Ajusta `push`/`pull` y shift registers para empaquetar datos eficientemente.
2. **Utiliza DMA.** Mantiene alimentadas las FIFOs sin bloquear la CPU.
3. **Divide funciones entre máquinas.** Un bloque puede generar reloj y otro manejar datos.
4. **Depura con `pioasm`.** El SDK incluye herramientas para validar el programa antes de compilar.
5. **Sincroniza con IRQ.** Usa interrupciones PIO para coordinar eventos con el firmware principal.

---

## Recursos adicionales

- Documentación oficial del **RP2040 Datasheet** (capítulo PIO).
- Ejemplos del **Pico SDK** (`pio/`): UART, I²C, PWM mejorado, DVI.
- Librerías comunitarias como **rp2040-pio-emulator** para pruebas en PC.

Dominar PIO te permitirá extender la Raspberry Pi Pico más allá de sus periféricos estándar y construir soluciones de tiempo real personalizadas.
