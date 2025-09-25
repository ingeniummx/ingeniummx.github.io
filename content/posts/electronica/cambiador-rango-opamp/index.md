---
title: "Cambiadores de rango con op-amps"
date: 2024-03-16T16:30:00+02:00
description: "Diseño de escaladores y cambiadores de rango utilizando amplificadores operacionales para acondicionamiento de señales."
menu:
  sidebar:
    name: Cambiadores de rango con op-amps
    identifier: cambiador-rango-opamp
    parent: electronica
    weight: 11
hero: images/cambiador-rango-opamp-cover.jpg
tags:
- Amplificadores operacionales
- Acondicionamiento de señal
- Instrumentación
- Diseño analógico
categories:
- Electrónica
- Analógico
math: true
---

Cuando un sensor entrega una señal fuera del rango de tu ADC o instrumento, necesitas un **cambiador de rango**. Con amplificadores operacionales puedes desplazar y escalar niveles para aprovechar mejor la resolución disponible.

---

## Objetivo de diseño

Queremos transformar una señal \(V_{in}\) en \(V_{out}\) según:

\[
V_{out} = a V_{in} + b
\]

donde \(a\) es la ganancia y \(b\) el offset. Un circuito sumador/restador con op-amp no inversor logra este comportamiento.

---

## Configuración típica

1. **Ganancia ajustable:** usa un amplificador no inversor con resistencias \(R_1\) y \(R_2\), ganancia \(1 + R_2/R_1\).
2. **Offset programable:** suma una referencia \(V_{ref}\) a través de un divisor y op-amp sumador.
3. **Fuente de referencia precisa:** TL431, DAC o referencia interna del microcontrolador filtrada.
4. **Rail-to-rail:** selecciona op-amps que operen dentro del rango de tu suministro y salida.

---

## Ejemplo práctico

Transformar 0–20 mA (convertido a 0–2,5 V sobre resistor de 125 Ω) a 0–5 V para un ADC.

1. Convierte corriente en voltaje: \(V_{sense} = 0\)–2,5 V.
2. Configura ganancia \(a = 2\) con \(R_2 = R_1\).
3. Offset \(b = 0\) (no requerido). Resultado: 0–5 V.

Para señales bipolares (por ejemplo -5 a +5 V a 0–3,3 V):

- Ganancia \(a = 0,33\).
- Offset \(b = 1,65\) usando referencia a mitad de 3,3 V.

---

## Consideraciones de precisión

- **Tolerancia de resistencias:** usa 0,1 % o mejor para errores <1 LSB.
- **Deriva térmica:** elige op-amps con bajo coeficiente y resistencias de película metálica.
- **Ruido y ancho de banda:** dimensiona filtros RC para limitar ruido antes del ADC.

---

## Protección

- Añade diodos de **clamp** y resistencias serie para proteger contra sobrevoltajes.
- Implementa **limitadores** (amplificadores con saturación) para mantener la señal dentro del rango esperado.

---

## Checklist

- [ ] ¿La referencia de offset es estable y filtrada?
- [ ] ¿El op-amp soporta el rango de entrada/salida?
- [ ] ¿La ganancia resultante cumple con los límites de tu ADC?
- [ ] ¿Añadiste filtrado anti-aliasing si el ADC es SAR?

Con estos lineamientos podrás diseñar cambiadores de rango fiables usando amplificadores operacionales.
