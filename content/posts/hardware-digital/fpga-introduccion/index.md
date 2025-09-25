---
title: "FPGAs"
date: 2024-03-16T19:00:00+02:00
description: "Introducción a las FPGAs, arquitectura interna, flujo de diseño y aplicaciones típicas."
menu:
  sidebar:
    name: FPGAs
    identifier: fpga-introduccion
    parent: hardware-digital
    weight: 1
hero: images/fpga-introduccion-cover.jpg
tags:
- FPGA
- Diseño digital
- HDL
- Electrónica
categories:
- Hardware digital
- Diseño digital
---

Las **FPGAs (Field Programmable Gate Arrays)** son dispositivos lógicos reconfigurables que permiten implementar hardware personalizado mediante lenguajes HDL. Ofrecen paralelismo masivo y tiempos deterministas que superan a los microcontroladores convencionales en tareas específicas.

---

## Arquitectura básica

- **Bloques lógicos configurables (CLB):** contienen LUTs, flip-flops y multiplexores.
- **Red de interconexión programable:** conecta LUTs y bloques especializados.
- **Bloques de memoria (BRAM) y UltraRAM:** almacenamiento de baja latencia.
- **DSP slices:** multiplicadores y acumuladores optimizados.
- **I/O programables:** soportan estándares LVDS, LVCMOS, SerDes, etc.

---

## Flujo de diseño

1. **Descripción HDL** (VHDL, Verilog, SystemVerilog) o herramientas de alto nivel (HLS, OpenCL).
2. **Sintetizado**: convierte el HDL en una red lógica.
3. **Implementación (place & route):** asigna recursos físicos y rutas de interconexión.
4. **Generación de bitstream**: archivo que configura la FPGA.
5. **Programación** mediante JTAG, SPI flash o procesadores integrados.

---

## Familias populares

| Fabricante | Serie | Rasgos |
| --- | --- | --- |
| Xilinx/AMD | Artix-7, Kintex-7, Zynq | Buena relación costo/rendimiento, SoC con ARM Cortex-A9. |
| Intel/Altera | Cyclone V, MAX 10, Arria | Opciones desde bajo costo hasta alto desempeño. |
| Lattice | iCE40, ECP5, Nexus | Bajo consumo, ideales para IoT y dispositivos portátiles. |
| Gowin | LittleBee, Arora | Alternativa económica con herramientas accesibles. |

---

## Aplicaciones

- **Procesamiento de señales** (radar, SDR, visión por computadora).
- **Puentes de comunicación** (PCIe, Ethernet, protocolos personalizados).
- **Control industrial** con tiempos deterministas.
- **Prototipado de ASIC** y emulación de hardware.

---

## Ecosistema de herramientas

- **Vivado / Vitis** (Xilinx), **Quartus Prime** (Intel), **Radiant / Diamond** (Lattice).
- Flujos open source: **Yosys + nextpnr**, **SymbiFlow**, **OpenFPGA**.
- Lenguajes de alto nivel: **Migen**, **nMigen**, **Chisel**, **SpinalHDL**.

---

## Consejos para principiantes

1. Empieza con placas accesibles (iCEstick, DE0-Nano, Arty A7).
2. Practica con diseños simples: contadores, PWM, UART.
3. Usa **simulación** (GHDL, ModelSim) antes de sintetizar para atrapar bugs.
4. Gestiona los **constraints** de tiempo y pines en archivos `.xdc` o `.sdc`.
5. Documenta versiones de herramientas; pequeñas diferencias pueden cambiar resultados.

Con esta base podrás adentrarte en el mundo de las FPGAs y escoger la plataforma adecuada para tus proyectos de diseño digital.
