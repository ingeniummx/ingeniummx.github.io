---
title: "Meshtastic explicado"
date: 2024-03-16T12:30:00+02:00
description: "Cómo funciona Meshtastic, hardware compatible y casos de uso para redes LoRa de largo alcance."
menu:
  sidebar:
    name: Meshtastic explicado
    identifier: meshtastic
    parent: comunicaciones
    weight: 1
hero: images/meshtastic-cover.jpg
tags:
- LoRa
- Mesh networks
- Comunicaciones
- IoT
categories:
- Comunicaciones
- IoT
---

**Meshtastic** es un firmware open source que transforma radios LoRa de bajo costo en redes malladas para mensajería y telemetría sin infraestructura celular. Es ideal para actividades al aire libre, resiliencia ante desastres y proyectos comunitarios.

---

## Arquitectura básica

- **Radios LoRa** basados en chip SX1262/SX1276 que operan en bandas ISM (433, 868, 915 MHz).
- **Firmware Meshtastic** sobre microcontroladores ESP32, nRF52 o RP2040.
- **Topología mesh**: cada nodo reenvía mensajes de manera asincrónica usando enrutamiento oportunista.
- **Aplicaciones móviles** (Android/iOS) y clientes CLI/desktop que se conectan vía Bluetooth o USB.

---

## Tipos de nodos

1. **Nodos personales:** dispositivos portátiles con pantalla (LilyGO T-Beam, T-Echo) que envían textos y ubicación GPS.
2. **Nodos de infraestructura:** placas alimentadas continuamente para ampliar cobertura (antenas externas, alta potencia).
3. **Nodos sensores:** integran telemetría (temperatura, humedad, relés) y reportan periódicamente.

---

## Configuración inicial

1. Instala el firmware con **Meshtastic Flasher** o `meshtastic --flash`.
2. Empareja el dispositivo con la app móvil mediante Bluetooth o USB.
3. Ajusta **canal, región y potencia** según normativa local (duty cycle, ERP).
4. Define roles (router, client) y habilita características como MQTT bridge o almacenamiento en tarjeta SD.

---

## Funciones destacadas

- **Mensajería encriptada** con AES-CTR y claves compartidas.
- **Posicionamiento GPS** y compartición de coordenadas con iconos personalizados.
- **Bridge MQTT/HTTP** para integrar con servidores o dashboards remotos.
- **Telemetría extendida** a través de plugins Python (`meshtastic-python`).

---

## Casos de uso

- Equipos de senderismo, ciclismo o rescate que requieren comunicación sin cobertura celular.
- Redes comunitarias de alerta temprana (incendios, inundaciones).
- IoT rural con nodos alimentados por energía solar.
- Eventos temporales donde se necesita mensajería descentralizada.

---

## Buenas prácticas

1. Utiliza **antenas calibradas** y respeta la polarización para maximizar alcance.
2. Configura **intervalos de retransmisión** adecuados para evitar congestión en mallas densas.
3. Documenta **claves y canales** en un gestor seguro para tu equipo.
4. Mantén el firmware actualizado y participa en la comunidad de GitHub/Discord para aprovechar nuevas funciones.

Con esta visión podrás planear una red Meshtastic confiable y adaptarla a tus necesidades de comunicación de largo alcance.
