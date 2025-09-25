---
title: "Encoders"
date: 2024-03-16T14:00:00+02:00
description: "Principios de funcionamiento de los encoders rotativos y lineales, tipos y criterios de selección."
menu:
  sidebar:
    name: Encoders
    identifier: encoders
    parent: sensores
    weight: 1
hero: images/encoders-cover.jpg
tags:
- Encoders
- Control de movimiento
- Sensores
- Automatización
categories:
- Sensores
- Control de movimiento
math: true
---

Los **encoders** convierten movimiento en señales eléctricas para medir posición, velocidad o dirección. Son fundamentales en robótica, CNC y servo sistemas.

---

## Clasificación principal

| Tipo | Principio | Resolución típica | Ventajas | Desventajas |
| --- | --- | --- | --- | --- |
| Incremental óptico | Interrupción de luz LED-fotorreceptor | 100–10.000 PPR | Alta precisión, señales cuadratura. | Sensibles al polvo, requieren referencia absoluta externa. |
| Incremental magnético | Sensor Hall/magnetorresistivo | 32–2048 PPR | Robustos, toleran suciedad. | Menor precisión angular. |
| Absoluto óptico | Código Gray en disco | 10–20 bits | Posición única sin homing. | Costosos, tamaño mayor. |
| Absoluto magnético | Sensor Hall 3D + ASIC | 12–16 bits | Compactos, soportan vibración. | Requieren calibración precisa del imán. |
| Lineales (regla óptica/magnética) | Codificación incremental o absoluta | 1–10 µm | CNC y metrología. | Instalación compleja. |

---

## Señales y conexiones

- **Cuadratura A/B**: permite detectar dirección y multiplicar la resolución x4 con flancos.
- **Index (Z)**: referencia una vuelta completa.
- **Salida push-pull u open-collector**: elige drivers compatibles con tu PLC o microcontrolador.
- **Interfaces absolutas**: SSI, BiSS-C, CANopen, EtherCAT, I²C/SPI en modelos compactos.

---

## Seleccionar un encoder

1. **Resolución necesaria**: determina pulsos por revolución (PPR) o bits. Considera reducción mecánica y microstepping.
2. **Velocidad máxima**: verifica frecuencia de salida \(f = \text{PPR} \times \text{RPM} / 60\) para dimensionar entradas.
3. **Ambiente**: polvo, vibración, temperatura. Escoge IP adecuado y sellado.
4. **Montaje**: eje sólido, hueco, sin rodamientos (kit encoder), acople flexible.
5. **Protocolo**: compatibilidad con controladores existentes.

---

## Aplicaciones típicas

- **Servomotores AC/DC** con control de posición.
- **Robots móviles** para odometría diferencial.
- **Impresoras 3D y CNC** para realimentar husillos o camas.
- **Instrumentación** en metrología y mesas de inspección.

---

## Buenas prácticas

1. **Alinea mecánicamente** el eje y usa acoples flexibles para evitar cargas radiales.
2. **Protege el cableado** con pares trenzados y blindaje conectado a tierra por un extremo.
3. **Implementa homing seguro** aun con encoders absolutos para validar límites físicos.
4. **Filtra ruido** con entradas diferenciales (RS-422) y filtros digitales en firmware.

Comprender estas variables te permitirá seleccionar el encoder adecuado y garantizar lecturas confiables en tus sistemas de control de movimiento.
