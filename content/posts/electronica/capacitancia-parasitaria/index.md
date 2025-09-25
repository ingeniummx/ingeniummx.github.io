---
title: "Capacitancia parasitaria en PCBs"
date: 2024-03-16T17:30:00+02:00
description: "Qué es la capacitancia parasitaria, cómo afecta señales rápidas y estrategias para minimizarla en PCBs."
menu:
  sidebar:
    name: Capacitancia parasitaria en PCBs
    identifier: capacitancia-parasitaria
    parent: electronica
    weight: 13
hero: images/capacitancia-parasitaria-cover.jpg
tags:
- PCB
- Señales rápidas
- Integridad de señal
- Diseño electrónico
categories:
- Electrónica
- Alta velocidad
math: true
---

La **capacitancia parasitaria** aparece de forma no intencional entre pistas, planos y componentes. En circuitos de alta velocidad puede degradar bordes, generar diafonía y alterar la respuesta en frecuencia.

---

## De dónde proviene

- **Pistas paralelas:** forman capacitores distribuidos cuando corren cercanas durante largos trayectos.
- **Pads y planos de referencia:** cada pad hacia un plano crea capacitancia que modifica la impedancia.
- **Componentes discretos:** encapsulados, cables y conectores poseen capacitancias internas.

Se aproxima con:

\[
C = \varepsilon \frac{A}{d}
\]

donde \(A\) es el área de superposición, \(d\) la distancia y \(\varepsilon\) la permitividad.

---

## Efectos en circuitos

- **Aumento del tiempo de subida** en señales digitales: \(t_r \approx 2,2 RC\).
- **Resonancias** junto con inductancias parásitas formando tanques LC.
- **Diafonía (crosstalk)** entre trazas paralelas.
- **Alteración de filtros** analógicos o osciladores LC.

---

## Cómo mitigarlo

1. **Controla la geometría:** mantén separación suficiente entre pistas de alta velocidad.
2. **Usa planos de referencia sólidos** para definir impedancia y reducir diafonía.
3. **Reduce áreas expuestas** en pads grandes mediante ventanas en solder mask.
4. **Acorta leads y cables** en prototipos para disminuir capacitancia añadida.
5. **Aplica terminaciones adecuadas** (series o en paralelo) para amortiguar efectos.

---

## Herramientas de análisis

- Simuladores de integridad de señal (HyperLynx, SiSoft, Keysight ADS).
- Calculadoras de impedancia y capacitancia en línea.
- Medición con **VNA** o **TDR** para validar prototipos.

---

## Checklist de diseño

- [ ] ¿Las pistas críticas siguen reglas de separación e impedancia?
- [ ] ¿Los planos de retorno están continuos y sin slots?
- [ ] ¿Se minimizaron stubs en vías y pads?
- [ ] ¿Se verificó la respuesta mediante simulación o medición?

Controlar la capacitancia parasitaria te permitirá mantener señales limpias y reproducibles en tus PCBs de alta velocidad.
