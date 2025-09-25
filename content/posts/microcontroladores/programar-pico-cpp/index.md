---
title: "Programar la Pico en C++"
date: 2024-03-16T13:30:00+02:00
description: "Pasos para configurar el Pico SDK y compilar proyectos en C/C++ para la Raspberry Pi Pico y Pico 2 sin Arduino."
menu:
  sidebar:
    name: Programar la Pico en C++
    identifier: programar-pico-cpp
    parent: microcontroladores
    weight: 4
hero: images/programar-pico-cpp-cover.jpg
tags:
- Raspberry Pi Pico
- C++
- Pico SDK
- Herramientas de desarrollo
categories:
- Microcontroladores
- Software
---

Programar la Raspberry Pi Pico directamente en C/C++ ofrece máximo control sobre el hardware y mejor desempeño que las capas de compatibilidad Arduino. Estos son los pasos clave para configurar el entorno oficial **Pico SDK** en Windows, Linux o macOS.

---

## Requisitos previos

- **Toolchain Arm GCC** (`arm-none-eabi-gcc` y `newlib`).
- **CMake ≥ 3.13** y **Ninja** o Make.
- **Python 3** para scripts auxiliares.
- **Git** para clonar el SDK y submódulos.

En Windows se recomienda usar **WSL** o el instalador oficial que empaqueta todas las dependencias.

---

## Instalación del Pico SDK

```bash
mkdir -p ~/pico && cd ~/pico
git clone -b master https://github.com/raspberrypi/pico-sdk.git
cd pico-sdk
git submodule update --init
export PICO_SDK_PATH=$PWD
```

Añade la variable `PICO_SDK_PATH` a tu `.bashrc` o script de entorno.

Para la **Pico 2 (RP2350)** habilita el flag `PICO_PLATFORM=rp2350` o usa la rama `sdk-2.0` que soporta el nuevo silicio.

---

## Crear un proyecto base

```bash
cd ~/pico
git clone https://github.com/raspberrypi/pico-examples.git
cd pico-examples/blink
mkdir build && cd build
cmake -DPICO_BOARD=pico_w ..
ninja
```

Generarás archivos `.uf2` y `.elf`. Copia el `.uf2` a la unidad montada cuando la Pico esté en modo BOOTSEL.

---

## Estructura recomendada

```
my-project/
├── CMakeLists.txt
├── cmake/
│   └── pico_sdk_import.cmake
├── include/
├── src/
│   └── main.cpp
└── build/
```

- `pico_sdk_import.cmake` apunta al SDK sin necesidad de variables globales.
- Define **targets** específicos (`pico_add_extra_outputs`, `pico_enable_stdio_usb`).

---

## Depuración y pruebas

1. Conecta un **debug probe** (Picoprobe, CMSIS-DAP) al puerto SWD.
2. Usa **OpenOCD** con el archivo `interface/picoprobe.cfg` y `target/rp2040.cfg`.
3. Inicia **gdb-multiarch** o `arm-none-eabi-gdb` para setear breakpoints y ver registros.
4. Aprovecha **pico_unit** o frameworks como **Unity** para pruebas unitarias en host.

---

## Buenas prácticas

- Habilita **std::span**, `-fno-exceptions` y LTO para reducir tamaño si no necesitas RTTI.
- Documenta la versión del SDK en `cmake/pico_sdk_import.cmake`.
- Crea **targets separados** para Pico y Pico W con diferentes opciones de Wi-Fi/Bluetooth.
- Automatiza builds con GitHub Actions usando contenedores oficiales `raspberrypi/pico-sdk`.

Con esta configuración tendrás un flujo profesional para desarrollar en C/C++ sobre la Raspberry Pi Pico y sus variantes sin depender de Arduino.
