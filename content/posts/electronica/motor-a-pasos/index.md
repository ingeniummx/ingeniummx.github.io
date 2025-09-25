---
title: "¿Qué es un motor a pasos y cómo aprovecharlo?"
date: 2024-03-15T09:00:00+02:00
description: "Conceptos clave para entender los motores a pasos: tipos, funcionamiento, drivers, aplicaciones y criterios para elegir el modelo correcto."
menu:
  sidebar:
    name: Motores a pasos
    identifier: motor-a-pasos
    parent: electronica
    weight: 8
hero: images/motor-a-pasos-cover.jpg
tags:
- Motores a pasos
- CNC
- Impresión 3D
- Control de motores
- Electrónica de potencia
categories:
- Electrónica
- Control de movimiento
- Mecatrónica
---

Los **motores a pasos** (stepper motors) convierten pulsos eléctricos en **movimientos angulares discretos**. En cada paso el eje gira un ángulo fijo (1,8°, 0,9°, etc.), por lo que podemos posicionar cargas sin retroalimentación mecánica adicional.

---

## ¿Cómo funciona un motor a pasos?

Un motor a pasos típico tiene:

- **Estator** con varias bobinas agrupadas en fases.
- **Rotor** dentado con imanes permanentes o material ferroso.
- **Secuencia de excitación**: aplicar corriente a las bobinas en un orden preciso genera un campo magnético rotatorio que “arrastra” al rotor.

Cada vez que cambiamos la fase energizada el rotor avanza un **paso**. Si completamos \(N\) pasos obtenemos una vuelta completa, y podemos calcular los pasos por vuelta como:

\[
\text{pasos por vuelta} = \frac{360^{\circ}}{\text{ángulo por paso}}
\]

Por ejemplo, un motor de 1,8° necesita 200 pasos para girar 360°.

---

## Modos de operación

- **Pasos completos**: energiza una fase a la vez. Ofrece el par nominal pero la resolución está limitada al ángulo por paso.
- **Medios pasos**: alterna entre una fase y dos fases activas. Duplica la resolución (1,8° → 0,9°) con ligero rizado de par.
- **Microstepping**: el driver modula corrientes senoidales para dividir cada paso en hasta 1/256. Reduce vibraciones y resonancias, ideal para impresoras 3D y CNC pequeños.

---

## Tipos de motores a pasos

| Tipo | Características | Ventajas | Desventajas | Casos típicos |
| --- | --- | --- | --- | --- |
| **Reluctancia variable** | Rotor dentado sin imanes. | Construcción económica, respuesta rápida. | Bajo par y fácil pérdida de pasos. | Instrumentación, actuadores ligeros. |
| **Imán permanente** | Rotor con imanes axiales. | Par moderado con electrónica simple. | Tamaño reducido, limitado en velocidad. | Escáneres, cámaras, actuadores lineales. |
| **Híbrido (PM + reluctancia)** | Combina imanes y dientes finos (1,8° o 0,9°). | Alto par/volumen, buena precisión. | Precio y necesidad de drivers más finos. | CNC, impresoras 3D, robótica ligera. |

También encontrarás configuraciones **unipolares** (cada fase dividida en dos enrollados con derivación central, fáciles de manejar con transistores o ULN2003) y **bipolares** (una bobina por fase, requieren puentes H pero ofrecen más par por cobre).

---

## ¿Dónde se usan los motores a pasos?

- **Impresoras 3D y CNC de escritorio**: posicionamiento de ejes XYZ y extrusores.
- **Robótica y automatización ligera**: pórticos pick-and-place, dispensadores de componentes.
- **Instrumentación y medición**: control de válvulas, galvanómetros digitales, ópticas.
- **Electrónica de consumo**: unidades de disquete, lentes de cámaras, lectores de tarjetas.

Su mayor fortaleza es el **posicionamiento abierto (open-loop)** con buena repetibilidad sin necesidad de encoders.

---

## Drivers y técnicas de control

Para moverlos correctamente necesitas un **driver** que entregue corriente controlada:

- **ULN2003 / ULN2803**: matrices Darlington para motores unipolares pequeños (28BYJ-48). Control tipo paso a paso desde un microcontrolador.
- **L298N / L293D**: puentes H clásicos. Funcionan, pero son ineficientes para corrientes altas.
- **A4988, DRV8825, TMC2208/2209**: drivers de micropasos con regulación de corriente por chopping, ideales para motores bipolares de 1–2 A.
- **Drivers industriales (Leadshine, Gecko, etc.)**: aceptan señales STEP/DIR o comunicación digital y pueden trabajar a decenas de voltios y amperios.

Buenas prácticas de control:

1. **Regular la corriente**: el par depende de la corriente RMS, no del voltaje. Ajusta los potenciómetros o usa drivers con autoajuste.
2. **Aumentar el voltaje de alimentación**: dentro del rango del driver, un voltaje más alto vence la inductancia y mantiene el par a alta velocidad.
3. **Rampas de aceleración/desaceleración**: evita perder pasos al arrancar o detener cargas inerciales.
4. **Evitar resonancias**: prueba diferentes microsteppings y añade amortiguadores o correas para suavizar vibraciones.

---

## Cómo elegir el motor correcto

1. **Par requerido**
   - Calcula el par de carga (Nm o kg·cm). Añade un 30–50 % de margen para fricción y aceleración.
2. **Ángulo por paso / resolución**
   - ¿Necesitas 200 pasos por vuelta (1,8°) o 400 pasos (0,9°)? Considera el microstepping disponible.
3. **Tamaño y estándar NEMA**
   - NEMA 17, 23, 34… indica el tamaño de la brida en pulgadas ×10. A mayor tamaño, más par.
4. **Corriente nominal y resistencia de bobina**
   - Define el driver y la fuente. Motores de baja resistencia funcionan mejor con drivers de corriente constante.
5. **Velocidad máxima**
   - Revisa curvas par–velocidad. El par cae a medida que aumenta la velocidad, especialmente en motores grandes con inductancia alta.
6. **Tipo de eje y acople**
   - Ejes lisos, estriados, con doble salida. Facilita el montaje en poleas, acoples flexibles o husillos.
7. **Ambiente y ciclo de trabajo**
   - Temperaturas elevadas o operación 24/7 requieren revisar aislamiento, ventilación y límites térmicos.

---

## Checklist rápido antes de comprar

- [ ] ¿El driver soporta la corriente RMS del motor?
- [ ] ¿La fuente de alimentación entrega el voltaje y corriente suficientes?
- [ ] ¿Incluiste rampas de aceleración en el firmware?
- [ ] ¿El montaje mecánico evita cargas laterales en el eje?
- [ ] ¿Necesitas un encoder o final de carrera como respaldo?

---

## Consejos finales

- **Protege tus bobinas**: usa diodos flyback o drivers con protección integrada.
- **Mide la temperatura** durante las pruebas. Un motor a pasos puede operar caliente (70–80 °C superficial), pero si no puedes tocarlo más de 2 segundos, revisa la corriente.
- **Cuida el cableado**: pares trenzados y blindaje reducen interferencias cuando el cable es largo.
- **Documenta la secuencia de cables**: usa etiquetas o conectores para evitar errores de conexión.

Con una selección adecuada y un driver bien configurado, los motores a pasos ofrecen **precisión repetible, costos razonables** y un ecosistema maduro para proyectos de automatización, robótica y fabricación digital.

