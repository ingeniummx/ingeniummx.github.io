---
title: "Sensores Hall"
date: 2024-03-16T14:30:00+02:00
description: "Funcionamiento de los sensores de efecto Hall, configuraciones lineales y digitales y aplicaciones en motores y encoders."
menu:
  sidebar:
    name: Sensores Hall
    identifier: sensores-hall
    parent: sensores
    weight: 2
hero: images/sensores-hall-cover.jpg
tags:
- Sensores Hall
- Motores
- Encoders
- Magnetismo
categories:
- Sensores
- Control de movimiento
math: true
---

El **efecto Hall** describe la diferencia de potencial que aparece cuando un conductor con corriente atraviesa un campo magnético. Los sensores Hall aprovechan este fenómeno para medir posición, velocidad o corriente sin contacto mecánico.

---

## Tipos principales

1. **Hall digital tipo interruptor:** salida on/off con histéresis. Usados en detección de proximidad, finales de carrera o conmutación de motores BLDC.
2. **Hall latch (bipolares):** cambian de estado al detectar polos norte/sur, ideales para codificadores magnéticos.
3. **Hall lineales:** entregan una tensión proporcional al campo \(V_{out} = V_{ref} + S \cdot B\), donde \(S\) es la sensibilidad.
4. **Sensores Hall de corriente:** combinan un conductor y núcleo magnético para medir corriente en bus.

---

## Aplicaciones en motores

- **Conmutación BLDC:** tres sensores ubicados a 120° eléctricos generan la secuencia de activación de fases.
- **Detección de rotor en motores paso a paso híbridos** cuando se requiere feedback adicional.
- **Protección contra sobrecorriente** en drivers mediante Hall lineales integrados.

---

## Integración en encoders

- Encoders magnéticos como **AS5048**, **MA730** o **TLE5012** usan un imán diametral y sensores Hall 2D/3D.
- La resolución depende del ASIC (hasta 14 bits) y la alineación axial del imán.
- Proporcionan interfaces SPI, I²C, PWM o ABI (A/B/Z) compatibles con sistemas existentes.

---

## Diseño con sensores Hall

1. **Colocación del imán:** asegura una distancia uniforme. Usa imanes diametrales o multipolares según la aplicación.
2. **Blindaje y ruido:** filtra con capacitores y protege contra campos externos fuertes.
3. **Alimentación estable:** muchos sensores usan referencias internas de 2,5 V; añade bypass de 100 nF + 1 µF.
4. **Calibración:** aplica offsets y escalas vía firmware para corregir tolerancias.

---

## Selección del sensor

- **Rango de campo:** elige un dispositivo cuyo rango abarque tu campo máximo sin saturación.
- **Sensibilidad y ruido:** evalúa densidad espectral de ruido para mediciones precisas.
- **Temperatura:** para aplicaciones automotrices busca rangos -40 a 150 °C.
- **Salida:** digital open-drain, push-pull, analógica ratiométrica o PWM.

Comprender estas variables te permitirá integrar sensores Hall de forma confiable en sistemas de motores, encoders y medición de corriente.
