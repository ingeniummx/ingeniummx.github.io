---
title: "Elegir la ESP32 adecuada"
date: 2024-03-16T11:30:00+02:00
description: "Comparación entre los módulos y devkits ESP32 más comunes para seleccionar el ideal según conectividad y recursos."
menu:
  sidebar:
    name: Elegir la ESP32 adecuada
    identifier: elegir-esp32
    parent: microcontroladores
    weight: 2
hero: images/elegir-esp32-cover.jpg
tags:
- ESP32
- IoT
- Wi-Fi
- Bluetooth
categories:
- Microcontroladores
- IoT
---

La familia **ESP32** de Espressif incluye docenas de variantes con diferentes radios, memoria y encapsulados. Escoger el módulo correcto evita limitaciones de memoria, consumo excesivo o falta de certificaciones inalámbricas.

---

## Criterios de selección

1. **Conectividad inalámbrica.** ¿Necesitas Wi-Fi 2,4 GHz, Bluetooth LE, Classic o incluso 802.15.4?
2. **Memoria y almacenamiento.** Flash integrada (4–16 MB), PSRAM opcional para buffers gráficos o voz.
3. **GPIO disponibles.** Algunos módulos comparten pines con la antena o el cristal, reduciendo los I/O libres.
4. **Certificaciones.** Para productos finales busca módulos con FCC/CE y antena integrada.
5. **Consumo energético.** Evalúa modos de suspensión, corriente en deep sleep y voltaje operativo.

---

## Variantes populares

| Módulo | CPU | RAM/Flash | Radios | Características clave |
| --- | --- | --- | --- | --- |
| ESP32-WROOM-32 | Xtensa LX6 dual a 240 MHz | 520 KB + 4 MB flash | Wi-Fi + BT Classic/LE | DevkitC, amplio soporte, antena PCB. |
| ESP32-WROVER | Xtensa LX6 dual | 520 KB + 4 MB flash + 8 MB PSRAM | Wi-Fi + BT | Ideal para gráficos, cámaras o SSL pesado. |
| ESP32-C3 | RISC-V single 160 MHz | 400 KB + 4 MB flash | Wi-Fi + BT LE 5 | Bajo consumo, pin-to-pin con ESP8266. |
| ESP32-S3 | Xtensa LX7 dual 240 MHz | 512 KB + 8 MB flash/PSRAM | Wi-Fi + BT LE 5 | Vector instructions para IA, USB OTG. |
| ESP32-C6 | RISC-V single 160 MHz | 512 KB + 4 MB flash | Wi-Fi 6 + BT LE 5 + 802.15.4 | Thread/Matter listo, eficiente. |

---

## Devkits recomendados

- **ESP32-DevKitC:** Referencia básica para WROOM. Incluye convertidor USB-UART y fácil acceso a pines.
- **ESP32-S3-DevKitC-1:** Para proyectos de visión gracias a USB nativo y soporte para cámaras.
- **ESP32-C3-DevKitM-1:** Ideal para wearables y sensores de baja potencia.
- **LilyGO T-Display / T-Embed:** Añaden pantalla IPS, ranura microSD y batería LiPo integrada.

---

## Consejos prácticos

1. **Define primero la pila de software.** ESP-IDF, Arduino Core, MicroPython o circuitos con Matter pueden requerir memoria adicional.
2. **Revisa el pinout oficial.** Algunos GPIO no toleran 5 V o están reservados para arranque (GPIO0, GPIO2, GPIO15).
3. **Planea la antena.** Respeta el keep-out de la PCB o el conector U.FL para no degradar la potencia.
4. **Aprovecha los modos de bajo consumo.** Configura RTC, ULP y wake-up por GPIO o temporizador para aplicaciones alimentadas por batería.
5. **Certifica tu producto.** Usa módulos pre-certificados y documenta las pruebas de radiofrecuencia.

Con esta matriz podrás elegir el módulo ESP32 que mejor se ajuste a tu presupuesto, consumo y requisitos de conectividad.
