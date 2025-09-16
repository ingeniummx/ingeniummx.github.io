---
title: "Puente H con relevadores: controla un motor DC con Arduino"
date: 2025-09-16T10:00:00+02:00
description: "Controla el sentido de giro de un motor DC usando dos relevadores y un Arduino. Una forma simple de entender cómo funciona un puente H sin usar un driver especializado."
menu:
  sidebar:
    name: Puente H con relevadores
    identifier: h-bridge-relay
    parent: electronica
    weight: 4
tags:
- Puente H
- Arduino
- Relevadores
- Electrónica básica
- Control de motores
categories:
- Electrónica
- Proyectos
hero: images/puente-h-relevadores-cover.jpg
math: true

---


Un **puente H** es un circuito que permite invertir el sentido de giro de un motor de corriente directa (DC) al cambiar la polaridad de alimentación entre sus terminales. Se utiliza en aplicaciones donde es necesario controlar el movimiento en ambos sentidos, como en robótica o mecanismos reversibles.

En este proyecto vamos a construir un puente H sencillo utilizando un **módulo de 2 relevadores de 5 V**, muy común en kits de Arduino, para controlar un motor DC de 12 V.

---

## ¿Qué es un módulo de dos relevadores?

Un módulo de relevadores es una tarjeta con uno o más relevadores listos para ser controlados por un microcontrolador.  

Por ejemplo, un **módulo de 2 canales** funciona con nivel lógico bajo (*LOW level trigger*) a 5 V, cada canal consume unos 15–20 mA de corriente de control y puede conmutar cargas de hasta **AC 250 V 10 A** o **DC 30 V 10 A**.

Esto lo hace ideal para controlar pequeños motores, lámparas o cargas que requieren mayor corriente que la que un Arduino puede manejar directamente.


## Materiales necesarios

- 1 × Motor DC de 12 V  
- 1 × Módulo de 2 relevadores de 5 V  
- 1 × Fuente de 12 V para el motor  
- 1 × Arduino (o cualquier microcontrolador compatible)  
- Cables de conexión  
- Protoboard (opcional)  


## ¿Cómo funciona el cambio de dirección?

Para invertir el giro del motor necesitamos **cambiar la polaridad de alimentación**.  

Esto se logra **cruzando las conexiones** a través de los contactos **NO (Normalmente Abierto)** y **NC (Normalmente Cerrado)** de cada relevador.  
Cuando uno de los relevadores conmuta, invierte la polaridad aplicada al motor y con ello el sentido de giro.


## Conexiones

En el siguiente diagrama se muestra cómo se conecta el motor, la fuente de 12 V, el módulo de relevadores y el Arduino:

<div style="text-align:center">
  <img src="/images/puente-h-relevadores-diagrama.png" alt="Diagrama de conexión puente H con relevadores" style="max-width:100%; border-radius:12px;"/>
</div>

1. Los **comunes (C)** de los relevadores van conectados a los dos terminales del motor.  
2. Los contactos **NO y NC** se cruzan entre sí para que un lado del motor reciba +12 V y el otro GND (según cuál relevador se active).  
3. El módulo de relevadores se conecta al Arduino:  
   - IN1 → pin digital (ej. D7)  
   - IN2 → pin digital (ej. D8)  
   - VCC → 5 V  
   - GND → GND  


## 🔄 Estados de operación

El puente H hecho con relevadores tiene **cuatro combinaciones posibles** según la activación de los dos relevadores (IN1 e IN2):  

<div style="text-align:center">
  <img src="/images/puente-h-relevadores-estados.png" alt="Estados del puente H con relevadores" style="max-width:100%; border-radius:12px;"/>
</div>

| IN1 | IN2 | Estado del motor         | Explicación |
|-----|-----|--------------------------|-------------|
| 0   | 0   | **Apagado**              | Ambos relevadores inactivos → motor sin tensión. |
| 1   | 0   | **Gira en un sentido**   | Un relevador invierte la polaridad, aplicando +12 V en un borne y GND en el otro. |
| 0   | 1   | **Gira en el otro sentido** | Se invierte la polaridad al contrario. |
| 1   | 1   | **Apagado (frenado)**    | Ambos relevadores activos → los dos terminales del motor quedan al mismo potencial (+12 V o GND) y el motor se frena rápidamente. |

📌 **Nota:** estos módulos suelen ser *active LOW*, lo que significa que:  
- `INx = 0` → relevador **activado**  
- `INx = 1` → relevador **desactivado**

---

## 📜 Código de ejemplo (Arduino)

```cpp
int IN1 = 7;
int IN2 = 8;

void setup() {
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
}

// Gira un sentido 2s, luego al otro
void loop() {
  // Giro hacia adelante
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  delay(2000);

  // Giro hacia atrás
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  delay(2000);

  // Motor apagado (frenado)
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  delay(2000);
}
