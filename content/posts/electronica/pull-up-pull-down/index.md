---
title: "Pull-up vs pull-down"
date: 2024-03-16T09:00:00+02:00
description: "Cómo elegir y dimensionar resistencias pull-up y pull-down para entradas digitales confiables."
menu:
  sidebar:
    name: Pull-up vs pull-down
    identifier: pull-up-pull-down
    parent: electronica
    weight: 9
hero: images/pull-up-pull-down-cover.jpg
tags:
- Resistencias
- Entradas digitales
- Microcontroladores
- Diseño electrónico
categories:
- Electrónica
- Básico
math: true
---

Las entradas digitales en microcontroladores, FPGAs o sensores necesitan una referencia estable de nivel lógico cuando el pulsador o dispositivo externo está abierto. Para lograrlo usamos **resistencias pull-up y pull-down**. Saber cuándo elegir cada una evita lecturas erráticas y consumo innecesario.

---

## Qué hace una resistencia pull-up

Una **pull-up** conecta la entrada a \(V_{CC}\) mediante una resistencia. Cuando el botón está abierto la entrada lee un "1" lógico; al cerrar el contacto hacia tierra, la corriente fluye a través de la resistencia y el nodo cae a "0".

Ventajas principales:

- Permite usar interruptores a GND, que suelen tener menos ruido.
- Compatible con buses open-drain/open-collector (I²C, 1-Wire).
- Protege el pin ante cortos accidentales a tierra limitando la corriente a \(I = \frac{V_{CC}}{R}\).

Cuida que el nivel alto cumpla la ecuación:

\[
V_{INH} = V_{CC} - I_{leak} \cdot R_{PU}
\]

donde \(I_{leak}\) es la corriente de fuga del pin. Si el resultado baja del umbral lógico alto, reduce el valor de la resistencia.

---

## Qué hace una resistencia pull-down

Una **pull-down** amarra la entrada a tierra. El pin lee "0" en reposo y sube a "1" cuando se conecta a \(V_{CC}\).

Se usa cuando:

- El dispositivo externo entrega un nivel alto activo (sensores NPN open-collector, PLCs de 24 V con optoacopladores).
- Se requieren interruptores a \(V_{CC}\) por motivos de seguridad (por ejemplo, detectar cable cortado).
- El pin interno solo ofrece pull-up configurable y necesitas el estado contrario.

---

## Cómo dimensionar el valor

La resistencia debe ser lo suficientemente baja para vencer corrientes de fuga y EMI, pero alta para limitar consumo.

| Contexto | Rango típico |
| --- | --- |
| Buses I²C 3,3 V (100 kHz) | 2,2 kΩ – 4,7 kΩ |
| Entradas GPIO generales | 4,7 kΩ – 47 kΩ |
| Entradas de alta impedancia (comparadores) | 100 kΩ – 1 MΩ |

Si hay **capacitancia parásita** en el nodo, el tiempo de subida \(t_r = 2,2 R C\). A mayor R o C, más lento el flanco: considera reducir el valor o colocar un buffer Schmitt trigger.

---

## Pull-ups internas vs externas

La mayoría de microcontroladores incluyen pull-ups/pull-downs configurables (10 kΩ – 50 kΩ). Úsalas para prototipos, pero añade componentes externos cuando:

- Necesitas valores más precisos o fuertes (1 kΩ) para buses rápidos.
- El pin debe soportar más de \(V_{CC}\) (por ejemplo, 12 V con optoacopladores).
- Requieres tolerancia térmica y confiabilidad a largo plazo.

---

## Buenas prácticas

1. **Documenta la lógica activa** en esquemáticos y firmware. Evita invertir señales por error.
2. **Añade filtrado RC** en líneas expuestas a cables largos.
3. **Combina con protección ESD** mediante diodos TVS si el pin sale al exterior.
4. **Verifica el consumo en reposo**: con 3,3 V y una pull-down de 4,7 kΩ, el botón presionado consume ~0,7 mA.

Con estos criterios podrás definir rápidamente qué estrategia usar y garantizar que tus entradas digitales sean estables en cualquier entorno.
