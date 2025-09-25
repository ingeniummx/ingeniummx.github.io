---
title: "Protección contra inversión de polaridad"
date: 2024-03-16T17:00:00+02:00
description: "Estrategias para proteger PCBs ante inversión de polaridad usando diodos, MOSFET y controladores dedicados."
menu:
  sidebar:
    name: Protección contra inversión de polaridad
    identifier: proteccion-inversion
    parent: electronica
    weight: 12
hero: images/proteccion-inversion-cover.jpg
tags:
- Protección eléctrica
- MOSFET
- PCB design
- Fuentes de alimentación
categories:
- Electrónica
- Energía
---

Conectar una fuente al revés puede destruir componentes y pistas. Implementar **protección contra inversión de polaridad** es obligatorio en PCBs expuestas a usuarios finales o mantenimiento en campo.

---

## Métodos comunes

### Diodo en serie

- Coloca un diodo Schottky en serie con la entrada.
- Ventaja: simplicidad extrema.
- Desventaja: caída de tensión (~0,3–0,5 V) y pérdida de potencia.

### Diodo en paralelo + fusible

- Diodo TVS o rectificador en antiparalelo, fusible o PTC en serie.
- Si se invierte la polaridad, el diodo conduce y el fusible abre.
- Adecuado para fuentes de alto voltaje con usuarios experimentados.

### MOSFET de canal P

- Configura un MOSFET P-channel con el cuerpo diode apuntando hacia la carga.
- Ofrece baja caída de tensión (mΩ) y protección bidireccional limitada.
- Cuidado con el **V_GS max** al usar fuentes por encima de 20 V.

### MOSFET de canal N ideal

- Usa un MOSFET N con controlador de puerta ideal (por ejemplo, **LTC4365**, **LM5060**).
- Proporciona protección bidireccional, limitación de corriente y detección de sobrevoltaje.
- Recomendado en fuentes industriales y baterías Li-ion de alto corriente.

---

## Consideraciones de diseño

1. **Rango de voltaje:** dimensiona componentes para el máximo esperado más margen.
2. **Corriente nominal:** MOSFET con \(R_{DS(on)}\) bajo y disipación calculada.
3. **Respuesta térmica:** añade planos de cobre y vias para disipar calor.
4. **Diagnóstico:** LEDs o indicadores que alerten de polaridad invertida.
5. **Pruebas:** aplica inversión con fuentes limitadas en corriente para validar que no se dañe la placa.

---

## Layout PCB

- Coloca la protección cerca del conector de entrada.
- Usa pistas cortas y anchas para reducir inductancia.
- Añade TVS adicionales para transientes rápidos.
- Documenta claramente la polaridad en serigrafía y manuales.

Con estas estrategias podrás diseñar PCBs robustas que soporten errores de conexión sin comprometer el rendimiento.
