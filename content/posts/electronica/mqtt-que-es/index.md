---
title: "¿Qué es MQTT y por qué se usa tanto en IoT?"
date: 2025-09-18T10:00:00+02:00
description: "Introducción práctica al protocolo MQTT, cómo funciona su modelo publish/subscribe y cómo empezar con el broker Mosquitto."
menu:
  sidebar:
    name: MQTT en pocas palabras
    identifier: mqtt-que-es
    parent: electronica
    weight: 6
hero: images/mqtt-protocol-cover.jpg
tags:
- MQTT
- IoT
- Protocolos de comunicación
- Mosquitto
- Electrónica conectada
categories:
- Electrónica
- IoT
- Redes industriales
---

El **protocolo MQTT (Message Queuing Telemetry Transport)** se ha convertido en el *idioma común* de los proyectos **IoT**, tanto domésticos como industriales. Es **ligero**, funciona bien con enlaces inestables y permite que sensores, microcontroladores, dashboards y servicios en la nube intercambien datos sin enredarse en sockets manuales. Su modelo **publish/subscribe** gira alrededor de un broker que recibe, filtra y entrega mensajes, de los clientes que publican o se suscriben, y de los temas (*topics*) que organizan la información con rutas jerárquicas como `ingenium/sala/temperatura`.

---

## Arquitectura publish/subscribe (sin acoplamientos duros)

A diferencia de HTTP, el emisor no conoce al receptor. El **broker** se encarga de recibir lo que publica un cliente y reenviarlo a todos los suscritos al mismo *topic*.

<div style="text-align:center; margin: 1em 0;">
  <div style="border:2px dashed #bbb; padding:28px; width:80%; height:260px; margin:0 auto; display:flex; align-items:center; justify-content:center; color:#777;">
    <strong>PLACEHOLDER IMG:</strong>&nbsp; "mqtt-topology.png" — Diagrama publish/subscribe con nodos, broker central y topics
  </div>
  <p><em>Topología típica: sensores publican, dashboards y actuadores se suscriben.</em></p>
</div>

Gracias a esa capa intermedia es posible escalar con facilidad, sumar clientes sin tocar el código de los demás y separar responsabilidades; un ESP32 puede limitarse a enviar datos mientras que la visualización corre en otro entorno. También facilita la integración de sistemas heterogéneos, desde PLC y Raspberry Pi hasta servicios en la nube, bajo un mismo esquema de temas.

---

## Cómo fluye un mensaje MQTT

El flujo típico empieza con un cliente que publica en un tema específico. El broker recibe la carga, la etiqueta con el nombre del topic correspondiente y la reenvía a todos los clientes suscritos. Cada suscriptor decide qué hacer con el payload: actualizar una gráfica, encender un relevador o guardar el dato en una base de datos. Este ciclo puede repetirse en milisegundos y acepta metadatos como la **calidad de servicio (QoS)** y la **retención** de mensajes.

---

## Niveles de QoS (Quality of Service)

MQTT define tres niveles de QoS para balancear fiabilidad y consumo de red. El nivel 0 entrega un mensaje una sola vez sin confirmación, por lo que resulta ideal en telemetría frecuente donde perder un paquete no afecta. El nivel 1 solicita confirmaciones al destinatario y reenvía si no las recibe, lo que implica diseñar clientes idempotentes para ignorar duplicados. El nivel 2 añade un apretón de manos en cuatro pasos que garantiza una entrega única a costa de mayor latencia, reservado para escenarios críticos.

---

## Retención, sesiones y *last will*

El broker puede almacenar el último mensaje de un tema mediante la función *retained message*, que resulta útil para estados como `ingenium/lab/puerta`. También admite la persistencia de sesión para que los clientes recuperen sus suscripciones después de una reconexión. Por último, ofrece el mecanismo **Last Will & Testament (LWT)** que se publica automáticamente si un cliente se desconecta de forma inesperada y así se avisa al resto del sistema.

---

## Mosquitto: el broker libre más popular

[**Eclipse Mosquitto**](https://mosquitto.org/) es un broker MQTT **open source**, ligero y con paquetes listos para Linux, Windows y contenedores Docker. Es una elección ideal para instalarlo en una **Raspberry Pi**, en un servidor casero o incluso en ambientes de producción. En sistemas Debian o Ubuntu basta con actualizar los repositorios, instalar los paquetes `mosquitto` y `mosquitto-clients`, y habilitar el servicio para que el broker comience a escuchar en `tcp://localhost:1883`. Las herramientas que acompañan al paquete permiten realizar pruebas inmediatas al suscribirse y publicar mensajes en un mismo equipo.

---

## Seguridad básica (no lo dejes abierto)

Cuando Mosquitto se expone fuera de la red local conviene activar autenticación con usuarios y contraseñas generadas con `mosquitto_passwd`, habilitar TLS con certificados y configurar listas de control de acceso que limiten los temas disponibles para cada cliente. Estas capas evitan que un broker abierto se convierta en una puerta de entrada a la infraestructura.

<div style="text-align:center; margin: 1em 0;">
  <div style="border:2px dashed #bbb; padding:28px; width:80%; height:240px; margin:0 auto; display:flex; align-items:center; justify-content:center; color:#777;">
    <strong>PLACEHOLDER IMG:</strong>&nbsp; "mosquitto-security-layers.png" — Esquema con autenticación, TLS y ACLs
  </div>
  <p><em>Capas mínimas cuando el broker sale a Internet.</em></p>
</div>

---

## Buenas prácticas para tus proyectos IoT

Conviene definir desde el inicio una jerarquía de temas clara, por ejemplo `ingenium/<área>/<dispositivo>/<sensor>`, y documentar el formato del payload para mantener la coherencia entre clientes. Los mensajes retenidos resultan valiosos al reportar estados críticos, mientras que el nivel de QoS 1 asegura que los comandos importantes no se pierdan. El broker ofrece métricas que pueden integrarse con herramientas como **Prometheus** o **Telegraf**, y se puede automatizar su despliegue mediante **Docker** o servicios de **systemd** para garantizar que el sistema regrese a la vida después de un corte eléctrico.

---

## Conclusión

MQTT es la pieza que conecta sensores, actuadores y servicios sin que dependan unos de otros. Con un broker como **Mosquitto** puedes empezar rápido, crecer a decenas de dispositivos y reforzar la seguridad sin cambiar tu código base. Ideal para el proyecto **Ingenium MX** cuando necesites una columna vertebral de comunicaciones ligera y confiable.

