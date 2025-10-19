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

# Cómo elegir el módulo ESP32 ideal para tu PCB

La familia ESP32 es estándar en IoT por costo, Wi-Fi/BLE y ecosistema. Al pasar de prototipo a producto, la elección del **módulo** impacta RF, consumo, certificación y BOM. Esta guía resume criterios **técnicos, regulatorios y logísticos** para seleccionar con confianza.

## 1) Define requisitos del producto

- **Energía y consumo.** ¿Batería o línea? Filtra por *deep sleep* (≈5 µA en C3 hasta >20 µA en S3) y por picos de TX (500–700 mA).  
- **Conectividad.** Además de Wi-Fi 2.4 GHz, hay BLE 5.x (C3/S3), 802.15.4 para Thread/Matter (C6), e incluso variantes sin Wi-Fi (H2). Elige primero la **familia** por protocolo.  
- **Memoria y periféricos.** TLS pesado, UI, voz/visión → **PSRAM** + más flash (p.ej., WROVER, S3 8–16 MB). Sensores simples → 2–4 MB.  
- **Entorno.** Rango de temperatura, blindaje metálico y robustez EMI (p.ej., WROOM-32E/UE). Verifica versiones industriales.

## 2) Elige la familia ESP32

| Familia | CPU | Radios | Ventajas | Casos típicos |
|---|---|---|---|---|
| **ESP32 (WROOM/WROVER)** | Xtensa LX6 | Wi-Fi b/g/n, BT Classic + LE | Ecosistema maduro, opciones con PSRAM | Gateways, audio, TFT |
| **ESP32-S2/S3** | Xtensa (S3 con SIMD) | Wi-Fi (S3: + BLE) | USB OTG, aceleraciones para IA | HID, cámaras/LCD, edge-AI |
| **ESP32-C2/C3/C6** | RISC-V | C3: Wi-Fi + BLE; C6: + 802.15.4 | Bajo consumo, Matter/Thread | Batería, domótica |
| **ESP32-H2** | RISC-V | BLE + 802.15.4 | Ultra bajo consumo, sin Wi-Fi | Zigbee/Thread con pila |

Primero **familia por protocolo/consumo**; luego la **variante**.

## 3) Compara módulos concretos

| Módulo | Antena | Memoria típica | Certificación | Notas de diseño |
|---|---|---|---|---|
| **WROOM-32E** | PCB | 4 MB flash | FCC/CE/IC/… | Respetar *keep-out* ~15 mm en antena. |
| **WROOM-32UE** | U.FL | 4 MB flash | FCC/CE | Antena externa para chasis metálico o más alcance. |
| **WROVER-B** | PCB + PSRAM | 4 MB flash + 8 MB PSRAM | FCC/CE | Más alto y más consumo; ideal audio/imagen. |
| **S3-WROOM-1** | PCB | 8 MB flash (op. PSRAM) | FCC/CE | USB OTG, soporte LCD/cámara, SIMD. |
| **S3-MINI-1** | PCB compacta | 8 MB flash | FCC/CE | 13×19 mm; perfecto en PCBs densas. |
| **C3-MINI-1** | PCB | 4 MB flash | FCC/CE/SRRC | Bajo consumo; reemplazo moderno de ESP8266. |
| **C6-MINI-1** | PCB | 4 MB flash | (varía) | Wi-Fi 6 + Thread/Matter; validar disponibilidad. |
| **PICO-V3-02** | SiP c/antena | 8 MB flash | FCC/CE | QFN 7×7 soldable como IC; requiere stackup cuidado. |

**Claves:**
- **Antena & forma.** Antena PCB exige zona libre de cobre/masa bajo y alrededor; U.FL sube coste pero habilita antenas certificadas y mejor desempeño en gabinetes.  
- **Certificación.** Preferir módulos **pre-certificados**. Cambios en antena/cable pueden requerir pruebas parciales.  
- **Suministro.** Revisar stock (Mouser/Digi-Key), NRND y hoja de ruta.  
- **Térmica/EMI.** Blindaje mejora EMI pero eleva temperatura. Considerar ventilación/difusores.

## 4) Integración en la PCB (práctico)

- **Sigue los design guides de Espressif.** Stack-up, planos, filtros y *reference layouts*.  
- **RF limpia.** Plano de tierra continuo, *stitching vias* perimetral, sin trazas bajo antena; líneas a 50 Ω calculadas (si U.FL).  
- **Alimentación robusta.** LDO/buck con margen para picos de TX; **bulk capacitors** cerca de 3V3, desacoplos por riel.  
- **Protecciones y test.** TVS en USB/UART, *pull-ups* en EN/GPIO0, *test pads* para flashing/ATE.  
- **Boot & firmware.** Documenta straps (GPIO0/EN y familia-dependientes), OTA y plan de programación en fábrica.

## 5) Certificación y producción

- **Trazabilidad regulatoria.** Guarda datasheets, FCC ID, DoC, reportes.  
- **DFM/DFT.** *Fixture* de prueba, autodisgnóstico de radio/memoria/sensores.  
- **Longevidad.** Confirma compromiso de vida útil y alternativas pin-compatibles.

## Checklist antes de congelar diseño

- [ ] Familia elegida por **protocolos** y **consumo**.  
- [ ] Módulo con **certificaciones** de tus mercados.  
- [ ] **Layout RF** revisado (antenna keep-out, masa, 50 Ω).  
- [ ] **Fuente** dimensionada para picos de radio + *bulk*.  
- [ ] Estrategia de **flasheo/OTA**, **pruebas** y **actualizaciones** definida.

Con estas decisiones, reduces riesgo en certificación, maximizas rendimiento inalámbrico y facilitas el escalado a producción.
