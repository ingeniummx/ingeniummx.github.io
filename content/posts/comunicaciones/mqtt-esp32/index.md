---
title: "Controlar un LED con ESP32 y MQTT"
date: 2024-03-16T19:30:00+02:00
description: "Tutorial para controlar un LED desde un broker MQTT local o remoto usando ESP32 y un VPS."
menu:
  sidebar:
    name: Controlar un LED con ESP32 y MQTT
    identifier: mqtt-esp32
    parent: comunicaciones
    weight: 2
hero: images/mqtt-esp32-cover.jpg
tags:
- MQTT
- ESP32
- IoT
- Domótica
categories:
- Comunicaciones
- IoT
---

MQTT es un protocolo ligero de mensajería ideal para IoT. Con un ESP32 y un broker local o en un VPS puedes controlar actuadores desde cualquier lugar del mundo.

---

## Preparar el broker

### Opción local (Mosquitto)

```bash
sudo apt update
sudo apt install mosquitto mosquitto-clients
sudo systemctl enable --now mosquitto
```

### Opción VPS

1. Contrata un VPS ligero (1 vCPU, 1 GB RAM).
2. Instala Mosquitto y configura autenticación básica en `/etc/mosquitto/conf.d/seguro.conf`:

```
allow_anonymous false
password_file /etc/mosquitto/passwd
listener 8883
cafile /etc/letsencrypt/live/tu-dominio/fullchain.pem
keyfile /etc/letsencrypt/live/tu-dominio/privkey.pem
certfile /etc/letsencrypt/live/tu-dominio/fullchain.pem
```

3. Genera certificados TLS con Let’s Encrypt y crea usuarios con `mosquitto_passwd`.

---

## Firmware ESP32 (Arduino core)

```cpp
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "TuRed";
const char* password = "TuClave";
const char* mqtt_server = "broker.tudominio.com";
const int mqtt_port = 8883;

WiFiClientSecure espClient;
PubSubClient client(espClient);
const int ledPin = 2;

void callback(char* topic, byte* payload, unsigned int length) {
  String mensaje;
  for (unsigned int i = 0; i < length; i++) mensaje += (char)payload[i];
  digitalWrite(ledPin, mensaje == "ON" ? HIGH : LOW);
}

void reconnect() {
  while (!client.connected()) {
    if (client.connect("esp32-led", "usuario", "password")) {
      client.subscribe("casa/sala/led");
    } else {
      delay(2000);
    }
  }
}

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
  }
  espClient.setCACert("-----BEGIN CERTIFICATE-----\n...\n-----END CERTIFICATE-----\n");
  client.setServer(mqtt_server, mqtt_port);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) reconnect();
  client.loop();
}
```

Para conexiones sin TLS ajusta el puerto 1883 y elimina `setCACert`, pero prioriza seguridad en despliegues reales.

---

## Publicar desde cualquier lugar

- Usa `mosquitto_pub -h broker.tudominio.com -t casa/sala/led -m "ON"`.
- Implementa dashboards con **Node-RED**, **Home Assistant** o apps móviles como **MQTT Dash**.
- Para automatización, crea reglas en tu VPS que escuchen webhooks y publiquen en el topic.

---

## Buenas prácticas

1. **Segmenta topics** por ubicación/dispositivo (`casa/sala/led`).
2. **Habilita TLS y autenticación** para conexiones remotas.
3. **Implementa Last Will** para detectar desconexiones inesperadas.
4. **Monitorea** con `mosquitto_sub` o servicios como Grafana + Telegraf.

Con este flujo podrás encender y apagar un LED desde cualquier lugar del mundo usando MQTT y tu ESP32.
