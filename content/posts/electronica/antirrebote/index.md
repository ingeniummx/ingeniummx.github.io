---
title: "¿Qué es un circuito antirrebote?"
date: 2025-09-09T10:00:00+02:00
description: "Qué es el rebote en pulsadores y cómo eliminarlo con un filtro RC y una máquina de estados con millis() en microcontroladores."
menu:
  sidebar:
    name: ¿Qué es un circuito antirrebote?
    identifier: circuito-antirrebote
    parent: electronica
    weight: 2
hero: images/circuito-antirrebote-cover.jpg
tags:
- Pulsadores
- Antirrebote
- Arduino
- ESP32
- PIC
categories:
- Electrónica
- Básico
math: true

Los **pulsadores**, **switches** y **relés** están en todos lados.  
Cuando los conectamos a un microcontrolador (PIC, ESP32, Arduino, etc.) esperamos un **cambio de estado limpio** por cada pulsación. En la práctica no ocurre así: los contactos mecánicos **rebotan** durante unos milisegundos y generan **varias transiciones** antes de estabilizarse. A esto se le llama **rebote de contacto** (*contact bounce*).

<div style="text-align:center; margin: 1em 0;">
  <div style="border:2px dashed #bbb; padding:28px; width:80%; height:220px; margin:0 auto; display:flex; align-items:center; justify-content:center; color:#777;">
    <strong>PLACEHOLDER IMG:</strong>&nbsp; "button-ideal-vs-bounce.png" — Señal ideal vs con rebote
  </div>
  <p><em>Ideal vs. real: sin rebote (arriba) vs. con rebote (abajo)</em></p>
</div>

En aplicaciones lentas (encender una lámpara) el rebote pasa desapercibido.  
En **sistemas digitales** (contadores, menús, teclados) puede convertirse en **pulsaciones falsas**.

---

## ¿Por qué rebotan los contactos?

Por **masa, elasticidad y vibración** de las piezas metálicas. Al presionar o soltar, los contactos **no** se quedan quietos al primer intento: golpean y “tiemblan” durante **1–20 ms** (depende de tipo/calidad, desgaste, humedad, temperatura).

---

## Pull-up vs Pull-down (y dónde aparece el rebote)

Hay dos formas comunes de cablear un botón:

- **Pull-up:** el pin está en `1` por defecto (resistencia a Vcc). Al **presionar**, cae a `0`.
  <div style="text-align:center; margin: 1em 0;">
    <div style="border:2px dashed #bbb; padding:28px; width:80%; height:200px; margin:0 auto; display:flex; align-items:center; justify-content:center; color:#777;">
      <strong>PLACEHOLDER IMG:</strong>&nbsp; "button-pullup-bounce.png" — Esquema + forma de onda (alto→bajo con rebote)
    </div>
    <p><em>Pull-up: rebote al ir de alto a bajo</em></p>
  </div>

- **Pull-down:** el pin está en `0` por defecto (resistencia a GND). Al **presionar**, sube a `1`.
  <div style="text-align:center; margin: 1em 0;">
    <div style="border:2px dashed #bbb; padding:28px; width:80%; height:200px; margin:0 auto; display:flex; align-items:center; justify-content:center; color:#777;">
      <strong>PLACEHOLDER IMG:</strong>&nbsp; "button-pulldown-bounce.png" — Esquema + forma de onda (bajo→alto con rebote)
    </div>
    <p><em>Pull-down: rebote al ir de bajo a alto</em></p>
  </div>

---

# Cómo eliminar el rebote (antirrebote)

Nos quedamos con **dos** estrategias sencillas y efectivas:  
**(1) Hardware con RC** y **(2) Software con máquina de estados usando `millis()`**.

## 1) Antirrebote por **hardware** con filtro RC

Vamos a suponer un rebote de **20 ms** para simplificar. La idea es que el filtro sea **un poco más lento** que ese rebote para que lo “suavice”.

- **Esquema (pull-up típico):** usa un **pull-up** a Vcc, el **botón a GND**, y coloca un **condensador del pin a GND**.  
- **Cálculo directo:** un RC se define por su constante de tiempo \( \tau = R \cdot C \). Queremos que **\(5\tau > 20\,\text{ms}\)**, así el pin no cruza varias veces el umbral durante el rebote.

**Ejemplo práctico (con 20 ms):**  
\[
R = 100\,\text{k}\Omega, \quad C = 47\,\text{nF} \quad \Rightarrow \quad \tau \approx 4.7\,\text{ms} \quad \Rightarrow \quad 5\tau \approx 23.5\,\text{ms}
\]

<div style="text-align:center; margin: 1em 0;">
  <div style="border:2px dashed #bbb; padding:28px; width:80%; height:220px; margin:0 auto; display:flex; align-items:center; justify-content:center; color:#777;">
    <strong>PLACEHOLDER IMG:</strong>&nbsp; "waveform-rc-debounce.png" — Rebote crudo vs salida filtrada por RC
  </div>
  <p><em>Arriba: rebote crudo. Abajo: el RC hace que el pin cruce el umbral una sola vez</em></p>
</div>

### Circuito recomendado (3.3 V o 5 V, pull-up)

- **Rpull-up = 100 kΩ** a Vcc  
- **SW1** a GND  
- **C = 47 nF** del pin a GND  
- (Opcional) **Rserie = 100 Ω** con el botón para limitar picos de corriente hacia el condensador

<div style="text-align:center; margin: 1em 0;">
  <div style="border:2px dashed #bbb; padding:28px; width:80%; height:220px; margin:0 auto; display:flex; align-items:center; justify-content:center; color:#777;">
    <strong>PLACEHOLDER IMG:</strong>&nbsp; "circuito-antirrebote-rc.png" — Esquema con Rpull-up, C al pin y Rserie
  </div>
  <p><em>Pull-up externo de 100 kΩ + C de 47 nF → 5τ ≈ 23.5 ms</em></p>
</div>

> **Nota:** El RC añade un **retardo** a la lectura. Si quieres más rapidez, reduce un poco \(R\) o \(C\) y valida que siga filtrando bien.

---

## 2) Antirrebote por **software** con `millis()` (no bloqueante)

Usaremos una **máquina de estados** muy simple: detecta un cambio crudo, abre una **ventana de estabilidad** y **confirma** el nuevo estado si pasan **~35 ms** sin más cambios. No bloquea la CPU.

```cpp
const int buttonPin = 2;             // INPUT_PULLUP si no usas RC
const int led = 13;
const unsigned long debounceMs = 35; // ventana de estabilidad

int stableState = HIGH;              // reposo en HIGH (pull-up)
int lastRead = HIGH;
unsigned long lastEdgeMs = 0;

void setup() {
  pinMode(buttonPin, INPUT_PULLUP);  // o INPUT si usas RC externo
  pinMode(led, OUTPUT);
}

void loop() {
  int reading = digitalRead(buttonPin);

  // 1) Cambio crudo → reinicia ventana
  if (reading != lastRead) {
    lastEdgeMs = millis();
    lastRead = reading;
  }

  // 2) Pasado debounceMs sin más cambios → confirma estado
  if ((millis() - lastEdgeMs) > debounceMs && reading != stableState) {
    stableState = reading;

    // Ejemplo: actuar al PRESIONAR (pull-up: HIGH->LOW)
    if (stableState == LOW) {
      digitalWrite(led, !digitalRead(led)); // toggle
    }

    // Si prefieres actuar al SOLTAR: (stableState == HIGH)
  }
}
```

> **Tip:** si usas **interrupciones**, es mejor llevar al pin de IRQ una **señal ya filtrada por RC** y, en la ISR, solo detectar el **flanco**.



## Conclusión y recomendaciones (en 8 puntos)

2) **Usa `millis()`** si no quieres tocar la PCB.  
3) **Combina ambos** (RC moderado + `millis()`) para producto **robusto**.  
4) **Valores de arranque:** RC **100 kΩ + 47 nF** (≈5τ = 23.5 ms), `debounceMs` **35 ms**.  
5) **Mide** el rebote con osciloscopio; si no, diseña para **~20 ms** y ajusta.  
6) **Evita `delay()`**; usa ventana **no bloqueante** con `millis()` o FSM.  
7) En PCB, deja **pads DNF** para R/C y pon el **C cerca del pin** (retorno GND corto).  
8) Si usas **IRQ**, filtra **antes** con RC y en la ISR solo detecta el **flanco**.
