---
title: "Cómo elegir el módulo ESP32 ideal para tu PCB"
date: 2024-06-05T10:00:00-06:00
description: "Guía detallada para comparar los módulos ESP32 disponibles y seleccionar el adecuado para integrar en una PCB personalizada."
menu:
  sidebar:
    name: Elegir módulo ESP32 para PCB
    identifier: seleccionar-modulo-esp32-pcb
    parent: microcontroladores
    weight: 8
tags:
- ESP32
- Hardware
- Diseño de PCB
- IoT
categories:
- Microcontroladores
- Diseño electrónico
---

La familia ESP32 se ha convertido en un estándar de facto para productos conectados gracias a su bajo costo, doble conectividad Wi-Fi/Bluetooth y un ecosistema maduro. Sin embargo, al momento de llevar un prototipo a producción surgen dudas sobre qué módulo conviene soldar en la PCB: ¿uno con antena integrada, un encapsulado SiP o quizá un módulo con PSRAM para visión artificial? Esta guía desglosa los criterios técnicos, regulatorios y logísticos que deberías evaluar antes de comprometerte con una variante concreta.

---

## Paso 1. Define los requisitos de tu producto

1. **Perfil de consumo y alimentación.** ¿Tu dispositivo funcionará con baterías de litio, pilas AA o alimentación permanente? Los módulos ESP32 varían en corriente en deep sleep (del orden de 5 µA en los ESP32-C3 hasta >20 µA en los ESP32-S3). Identificar un presupuesto energético te permitirá filtrar familias.
2. **Tipo de conectividad.** Además del Wi-Fi 2,4 GHz, los módulos recientes añaden Bluetooth Low Energy 5.x, Wi-Fi 6 (ESP32-C6) o IEEE 802.15.4 para Thread/Matter. Escoge la familia (ESP32, ESP32-S, ESP32-C, ESP32-H) que coincida con tus protocolos objetivo.
3. **Recursos de memoria y periféricos.** Aplicaciones con TLS pesado, reconocimiento de voz o pantallas gráficas se benefician de módulos con PSRAM y flash ampliada (ESP32-WROVER, ESP32-S3 con 8–16 MB). Para sensores o control sencillo, los módulos con 2–4 MB de flash son suficientes.
4. **Condiciones ambientales.** Algunos módulos vienen certificados para -40 °C a 105 °C y ofrecen encapsulados metálicos (ESP32-WROOM-32E/U). Para entornos industriales, revisa hojas de datos y versiones “-E” o “-U”.

---

## Paso 2. Selecciona la familia ESP32 adecuada

| Familia | Arquitectura | Radios | Ventajas clave | Casos de uso ideales |
| --- | --- | --- | --- | --- |
| **ESP32 (WROOM/WROVER)** | Xtensa LX6 dual core hasta 240 MHz | Wi-Fi 802.11 b/g/n, BT Classic + LE | Máxima compatibilidad, soporte maduro, PSRAM opcional | Pasarelas Wi-Fi, audio, displays TFT |
| **ESP32-S2/S3** | Xtensa LX7 (S3) con vectoriales | Wi-Fi 2,4 GHz + BT LE (S3) | USB OTG, instrucciones SIMD, seguridad mejorada | Interfaces humanas, visión embebida, IA de borde |
| **ESP32-C2/C3/C6** | Núcleo RISC-V single core | Wi-Fi 2,4 GHz + BT LE 5 (C3) + 802.15.4 (C6) | Bajo consumo, PPA óptimo, Matter/Thread | Sensores alimentados por batería, domótica Matter |
| **ESP32-H2** | RISC-V | BT LE 5 + 802.15.4 | Sin Wi-Fi, ultra bajo consumo | Redes Zigbee/Thread alimentadas por pila |

Dentro de cada familia existen módulos con distintos tamaños y antenas. Elige primero la familia que cubra tus protocolos y consumo; después afina la variante.

---

## Paso 3. Compara módulos disponibles

| Módulo | Antena | Memoria típica | Certificaciones | Notas de diseño |
| --- | --- | --- | --- | --- |
| **ESP32-WROOM-32E** | PCB integrada | 4 MB flash | FCC, CE, IC, TELEC | Requiere keep-out de 15 mm alrededor de la antena. Ideal para Wi-Fi clásico. |
| **ESP32-WROOM-32UE** | Conector U.FL | 4 MB flash | FCC, CE | Permite antena externa para cajas metálicas o alto alcance. Agrega línea de alimentación para el coaxial. |
| **ESP32-WROVER-B** | PCB + PSRAM | 4 MB flash + 8 MB PSRAM | FCC, CE | Encapsulado metálico, cuidado con el consumo y la altura. Perfecto para audio/imagen. |
| **ESP32-S3-WROOM-1** | PCB | 8 MB flash (opcional PSRAM) | FCC, CE | USB OTG nativo, soporte para LCD/cámaras, vectores para IA. |
| **ESP32-S3-MINI-1** | PCB compacta | 8 MB flash | FCC, CE | Huella reducida (13×19 mm). Ideal para wearables o PCBs densas. |
| **ESP32-C3-MINI-1** | PCB | 4 MB flash | FCC, CE, SRRC | Bajo consumo, pin-compatible con ESP32-MINI-1. Excelente para reemplazar ESP8266. |
| **ESP32-C6-MINI-1** | PCB | 4 MB flash | En proceso (2024) | Añade Wi-Fi 6 + Thread/Matter. Considera la disponibilidad y soporte de firmware. |
| **ESP32-PICO-V3-02** | Antena integrada, SiP | 8 MB flash | FCC, CE | Paquete QFN (7×7 mm) soldable como IC. Requiere PCB multicapa y ensamblado de alta precisión. |

### Consideraciones clave

- **Antena y factor de forma.** Los módulos con antena PCB exigen respetar la zona libre de cobre y componentes bajo la antena; de lo contrario, el rendimiento RF se degrada. Las versiones con U.FL añaden costo pero permiten antenas externas certificadas.
- **Certificaciones radio.** Ahorra tiempo de homologación eligiendo módulos con certificaciones regionales pre-aprobadas. Si modificas la antena (por ejemplo, cambiando el cable coaxial), podrías necesitar pruebas parciales de recertificación.
- **Cadena de suministro.** Revisa la disponibilidad en distribuidores globales (Mouser, Digi-Key) y el estado NRND de cada módulo. Espressif publica un Product Selector actualizado y notas de longevidad.
- **Temperatura y encapsulado.** Los módulos con blindaje metálico mejoran la inmunidad EMI pero elevan la temperatura. Evalúa ventilación y disipación si tu PCB trabaja en gabinetes cerrados.

---

## Paso 4. Integra el módulo en tu PCB

1. **Consulta los design guides oficiales.** Espressif ofrece archivos Gerber y guías de diseño para cada módulo, con recomendaciones de stack-up, planos de tierra y filtrado.
2. **Planifica el layout alrededor de la antena.** Mantén un plano de tierra sólido, vías de stitching y evita trazas paralelas a la antena. Para U.FL, sigue el ancho de línea calculado para 50 Ω.
3. **Gestiona la alimentación.** Incluye regulador LDO o buck capaz de entregar picos de 500–700 mA durante transmisiones Wi-Fi. Añade bulk capacitors cerca del pin 3V3.
4. **Protección y test.** Agrega TVS para ESD en USB/UART, resistencias de pull-up en GPIO0/EN y puntos de test para flasheo y validación en producción.
5. **Firmware y bootstraps.** Documenta la secuencia de pines de arranque (GPIO0, GPIO2, GPIO15 según la familia) y el modo de actualización OTA para reducir intervención en campo.

---

## Paso 5. Considera la certificación y producción

- **Documenta el uso de módulo pre-certificado.** Mantén el datasheet, reportes de pruebas y números de identificación (FCC ID, CE DoC) para demostrar conformidad.
- **Prepara pruebas de fabricación (DFM/DFT).** Incluye fixture de test y programa de auto diagnóstico para validar radio, memoria y sensores antes del envío.
- **Plan de soporte a largo plazo.** Identifica la hoja de ruta de Espressif: algunas familias tienen garantía de disponibilidad de 12–15 años. Evalúa alternativas pines-compatibles para contingencias.

---

## Checklist final antes de bloquear el diseño

- [ ] Familia ESP32 seleccionada según conectividad y consumo.
- [ ] Módulo elegido con certificaciones aplicables a tus mercados objetivo.
- [ ] Layout PCB validado con las guías oficiales y revisiones de antena.
- [ ] Régimen de alimentación dimensionado para picos de radio.
- [ ] Estrategia de programación, pruebas y actualizaciones definida.

Tomar decisiones conscientes en estas cinco etapas reducirá sorpresas en certificación, mejorará el rendimiento inalámbrico y facilitará el escalado a producción masiva de tu dispositivo basado en ESP32.
