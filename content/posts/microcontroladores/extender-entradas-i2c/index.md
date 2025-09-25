---
title: "Ampliar entradas digitales con I2C"
date: 2024-03-16T18:00:00+02:00
description: "Cómo extender las entradas digitales de la Raspberry Pi Pico usando expansores GPIO I2C como el MCP23017 o el PCF8575."
menu:
  sidebar:
    name: Ampliar entradas digitales con I2C
    identifier: extender-entradas-i2c
    parent: microcontroladores
    weight: 5
hero: images/extender-entradas-i2c-cover.jpg
tags:
- Raspberry Pi Pico
- I2C
- Expansores GPIO
- Hardware modular
categories:
- Microcontroladores
- IoT
---

Cuando el número de GPIO de la Raspberry Pi Pico no basta, puedes **ampliar entradas digitales** con expansores I2C. Estos integrados entregan 8, 16 o más pines configurables sin sacrificar recursos del microcontrolador.

---

## Expansores populares

| Chip | Bits | Voltaje | Características |
| --- | --- | --- | --- |
| MCP23017 | 16 | 1,8–5,5 V | Dos bancos de 8 bits, interrupciones configurables, pull-ups internos. |
| PCF8575 | 16 | 2,5–5,5 V | Entradas/salidas quasi-bidireccionales, ideal para lectura de botones. |
| TCA9555 | 16 | 2,3–5,5 V | Interrupciones separadas, compatible con 400 kHz. |
| SX1509 | 16 | 2,5–3,6 V | PWM por pin, debounce interno y lógica de interrupciones avanzada. |

---

## Conexión básica

1. Conecta SDA y SCL al bus I2C de la Pico (GPIO 4 y 5 por defecto) con **pull-ups de 4,7 kΩ**.
2. Alimenta el expansor con 3,3 V o el voltaje requerido.
3. Configura las direcciones mediante pines A0-A2.
4. Conecta pines de interrupción (INTA/INTB) a GPIO disponibles para detectar cambios sin sondeo constante.

---

## Ejemplo con MCP23017 en MicroPython

```python
from machine import I2C, Pin
from mcp23017 import MCP23017

i2c = I2C(0, scl=Pin(5), sda=Pin(4), freq=400000)
exp = MCP23017(i2c, address=0x20)
exp.setup(0, MCP23017.IN, pullup=True)
exp.setup(1, MCP23017.IN, pullup=True)

while True:
    estado0 = exp.input(0)
    estado1 = exp.input(1)
    # Procesa botones
```

En C/C++ utiliza la librería `hardware/i2c.h` y escribe a los registros `IODIRA`, `GPPUA` y `GPIOA`.

---

## Buenas prácticas

1. **Aísla dominios de voltaje** si conectas señales de 5 V; algunos expansores toleran 5 V sólo en entradas.
2. **Debounce por hardware o firmware** para evitar falsos positivos en botones.
3. **Gestiona interrupciones** leyendo ambos puertos para limpiar flags.
4. **Escala el bus**: I2C soporta hasta 8 dispositivos por dirección si usas diferentes direcciones.
5. **Considera SPI** si necesitas velocidades mayores; chips como MCP23S17 ofrecen interfaz alternativa.

Con expansores I2C podrás incrementar las entradas digitales de tu Pico sin rediseñar la placa principal.
