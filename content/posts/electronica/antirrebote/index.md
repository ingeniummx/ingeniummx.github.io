---
title: "¿Qué es un circuito antirrebote?"
date: 2025-09-09T10:00:00+02:00
description: "Qué es el rebote en pulsadores, cómo medirlo y cómo eliminarlo con un filtro RC y una máquina de estados con millis() en microcontroladores."
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
---

Los **pulsadores**, **switches** y **relés** están en todos lados.  
Cuando los conectamos a un microcontrolador (PIC, ESP32, Arduino, etc.) esperamos un **cambio de estado limpio** por cada pulsación. En la práctica no ocurre así: los contactos mecánicos **rebotan** durante unos milisegundos y generan **varias transiciones** antes de estabilizarse. A esto se le llama **rebote de contacto** (*contact bounce*).

<div style="text-align:center; margin: 1em 0;">
  <img src="images/button-ideal-vs-bounce.png" alt="Señal ideal vs señal con rebote" style="max-width:80%;">
  <p><em>Ideal vs. real: sin rebote (arriba) vs. con rebote (abajo)</em></p>
</div>

En aplicaciones lentas (encender una lámpara) el rebote pasa desapercibido.  
En **sistemas digitales** (contadores, menús, teclados) se transforma en **pulsaciones falsas**.

---

## ¿Por qué rebotan los contactos?

Por **masa, elasticidad y vibración** de las piezas metálicas. Al impactar, los contactos se separan y vuelven a tocar varias veces en pocos milisegundos. El rebote aparece **al presionar y al soltar**.  
Duración típica: **1–20 ms** (varía con tipo/calidad, desgaste, humedad, temperatura).

---

## Pull-up vs Pull-down (y dónde aparece el rebote)

Para leer un push button hay dos conexiones típicas:

- **Pull-up:** el pin está en `1` (resistencia a Vcc). Al **presionar**, cae a `0`.
  <div style="text-align:center; margin: 1em 0;">
    <img src="images/button-pullup-bounce.png" alt="Pulsador con pull-up" style="max-width:80%;">
    <p><em>Pull-up: rebote al ir de alto a bajo</em></p>
  </div>

- **Pull-down:** el pin está en `0` (resistencia a GND). Al **presionar**, sube a `1`.
  <div style="text-align:center; margin: 1em 0;">
    <img src="images/button-pulldown-bounce.png" alt="Pulsador con pull-down" style="max-width:80%;">
    <p><em>Pull-down: rebote al ir de bajo a alto</em></p>
  </div>

---

# Cómo eliminar el rebote (antirrebote)

Nos quedamos con **dos** estrategias claras y suficientes:  
**(1) Hardware con RC** y **(2) Software con máquina de estados usando `millis()`**.

## 1) Antirrebote por **hardware** con filtro RC

Vamos a **asumir** un rebote de **20 ms** para simplificar el diseño. La idea es que el filtro sea un poco más lento que ese rebote para que lo “suavice”.

- **Dónde va cada cosa (caso pull-up):** usa un **pull-up externo** a Vcc, **botón a GND**, y pon un **condensador del pin a GND**. Configura el pin como **INPUT** (sin pull-up interno) para que el RC lo defina todo.
- **Cálculo simple:** escogemos valores para cumplir aproximadamente \(5\tau > 20\,\text{ms}\). Con
  \[
  \tau = R \cdot C
  \]
  si usamos **R = 100 kΩ** y **C = 47 nF**, obtenemos \(\tau \approx 4.7\,\text{ms}\) y **5τ ≈ 23.5 ms**, suficiente para filtrar el rebote de 20 ms.

<div style="text-align:center; margin: 1em 0;">
  <img src="images/waveform-rc-debounce.png" alt="Señal con rebote vs señal filtrada por RC" style="max-width:80%;">
  <p><em>Arriba: rebote “crudo”. Abajo: el RC hace que el pin cruce el umbral solo una vez</em></p>
</div>

### Circuito práctico (3.3 V o 5 V, ejemplo pull-up)

- **Rpull-up = 100 kΩ** a Vcc (externa)  
- **SW1** a GND  
- **C = 47 nF** del pin a GND  
- (Opcional) **Rserie = 100 Ω** con el botón para limitar picos hacia el condensador

<div style="text-align:center; margin: 1em 0;">
  <img src="images/circuito-antirrebote-rc.png" alt="RC práctico con pull-up y C al pin" style="max-width:80%;">
  <p><em>Pull-up externo de 100 kΩ + C de 47 nF → 5τ ≈ 23.5 ms</em></p>
</div>

> **Nota:** El RC añade un **retardo** a la lectura del botón. Si necesitas que responda más rápido, baja un poco \(R\) o \(C\) y prueba que siga filtrando bien.

---

## 2) Antirrebote por **software** con `millis()` (no bloqueante)

Usamos una **máquina de estados** muy simple:  
1) Detecta un cambio (posible rebote).  
2) Abre una ventana de **estabilidad**.  
3) Confirma el nuevo estado cuando transcurre el tiempo sin más cambios.

```cpp
const int buttonPin = 2;             // si no usas RC, INPUT_PULLUP es lo más práctico
const int led = 13;
const unsigned long debounceMs = 35; // ~35 ms funciona bien en la mayoría de casos

int stableState = HIGH;              // reposo en HIGH con pull-up
int lastRead = HIGH;
unsigned long lastEdgeMs = 0;

void setup() {
  pinMode(buttonPin, INPUT_PULLUP);  // o INPUT si usas el RC con pull-up externo
  pinMode(led, OUTPUT);
}

void loop() {
  int reading = digitalRead(buttonPin);

  // 1) Cambio crudo → reinicia ventana de estabilidad
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

> **Tip:** si usas **interrupciones**, es mejor llevar al pin de IRQ una **señal ya filtrada por RC** y en la ISR solo detectar el flanco.

---

## Comparativa rápida

| Aspecto                 | Hardware (RC)                         | Software (`millis()`)                   |
|-------------------------|---------------------------------------|-----------------------------------------|
| Componentes externos    | Sí                                    | No                                      |
| Flexibilidad            | Fija (R y C)                          | Alta (ajuste en código)                 |
| Carga del MCU           | Nula                                  | Baja (no bloqueante)                    |
| Robustez EMI            | Alta (filtra en el borde)             | Media (depende de umbrales/ruido)       |
| Ideal para              | Entradas a interrupción, señal crítica| Prototipos, UI, firmware flexible       |

---

## Conclusión

Con **RC** en hardware (p. ej., **100 kΩ + 47 nF → 5τ ≈ 23.5 ms**) y **`millis()`** en firmware (~**35 ms**), cubres la gran mayoría de casos: señal **limpia**, código **simple** y comportamiento **predecible**.
