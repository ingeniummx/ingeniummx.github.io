---
title: "Cómo escoger baterías"
date: 2024-03-16T10:00:00+02:00
description: "Comparativa entre LiPo, LiFePO4, plomo-ácido y otras químicas para alimentar proyectos electrónicos."
menu:
  sidebar:
    name: Cómo escoger baterías
    identifier: escoger-baterias
    parent: electronica
    weight: 10
hero: images/escoger-baterias-cover.jpg
tags:
- Baterías
- Alimentación
- Energía portátil
- Power design
categories:
- Electrónica
- Energía
---

Elegir la batería correcta evita apagones, sobrecalentamientos y ciclos de vida cortos. Esta guía resume las químicas más comunes, sus parámetros clave y criterios para combinarlas con tu electrónica.

---

## Parámetros críticos

1. **Voltaje nominal y rango operativo.** Asegúrate de que la tensión se mantenga dentro del margen de tu electrónica y reguladores.
2. **Capacidad (mAh o Wh).** Calcula el consumo promedio y el pico. Multiplica por el tiempo deseado y agrega un 20–30 % de margen.
3. **Capacidad de descarga (C-rate).** Determina la corriente máxima continua y en ráfagas.
4. **Ciclo de vida.** Considera cuántos ciclos completos soporta antes de caer al 80 %.
5. **Temperatura y seguridad.** Verifica límites de operación y si necesitas circuitos de protección.

---

## Tabla comparativa

| Química | Voltaje por celda | Densidad energética | C-rate típica | Puntos fuertes | Precauciones |
| --- | --- | --- | --- | --- | --- |
| LiPo (Li-ion polímero) | 3,7 V (2,7–4,2 V) | Muy alta | 1C–35C | Ligera, entrega altas corrientes. | Necesita balanceo y protección contra sobrecarga/descarga. |
| LiFePO₄ | 3,2 V (2,5–3,6 V) | Media | 1C–5C | Segura, >2000 ciclos, estable térmicamente. | Voltaje por celda menor, requiere más celdas en serie. |
| Plomo-ácido (SLA, AGM) | 2,0 V (1,8–2,4 V) | Baja | 0,2C–1C | Económica, disponible en altos Ah. | Pesada, no tolera descargas profundas continuas. |
| NiMH | 1,2 V (1,0–1,4 V) | Media | 0,5C–5C | Robusta, fácil de cargar. | Autodescarga alta, requiere cargador inteligente. |
| 18650 Li-ion | 3,6 V (2,5–4,2 V) | Muy alta | 1C–10C | Modular, buena densidad. | Igual que LiPo: protección obligatoria. |

---

## Seleccionar el formato

- **Drones y robótica móvil:** LiPo por su alta potencia instantánea. Incluye monitorización individual de celdas y bolsas ignífugas.
- **IoT estacionario o respaldo:** LiFePO₄ por seguridad y longevidad. Combina con BMS y cargador CC/CV dedicado.
- **Prototipos económicos:** Pack de NiMH AA recargables o SLA pequeño. Aceptan maltrato y son fáciles de encontrar.
- **Aplicaciones industriales 24/7:** LiFePO₄ o Li-ion con celdas 18650 de calidad, gestionadas con BMS redundante y telemetría.

---

## Gestión y protección

1. **Incluye un BMS adecuado.** Asegura balanceo, corte por sobrecorriente y protección térmica.
2. **Dimensiona el cargador.** La corriente recomendada suele ser 0,5C para LiPo/Li-ion y 0,3C para LiFePO₄.
3. **Regulación de voltaje.** Usa convertidores buck/boost para entregar tensiones estables a tu circuito.
4. **Plan de almacenamiento.** LiPo al 40–60 % y en bolsas ignífugas; plomo-ácido siempre cargadas.
5. **Monitoreo en campo.** Integra medidores de coulomb, estimación de SoC y alarmas de temperatura.

---

## Checklist rápido

- [ ] ¿La química soporta la corriente pico de tu carga?
- [ ] ¿Tu BMS controla temperatura, balance y sobrecarga?
- [ ] ¿El cargador respeta curvas CC/CV o delta-peak según la química?
- [ ] ¿La caja o chasis incluye ventilación y protección mecánica?
- [ ] ¿Documentaste procedimiento de transporte y reciclaje?

Con estos pasos podrás seleccionar una batería que equilibre seguridad, autonomía y costo sin sorpresas en el laboratorio ni en campo.
