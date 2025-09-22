---
title: "Cómo instalar Ubuntu 24.04 en Windows 11 con WSL2 paso a paso"
date: 2025-09-15T10:00:00+02:00
description: "Guía paso a paso para instalar Ubuntu 24.04 en WSL2, configurar aplicaciones esenciales y trabajar con Visual Studio Code en Windows 11."
menu:
  sidebar:
    name: Ubuntu 24.04 en WSL
    identifier: ubuntu-wsl
    parent: linux
    weight: 3
tags:
- WSL2
- Ubuntu 24.04
- Windows 11
- VS Code
- Linux
- Linux en Windows
categories:
- Linux
- Desarrollo.
- Tutoriales
hero: images/wsl-ubuntu-vscode-cover.jpg
---

El **Subsistema de Windows para Linux (WSL2)** permite correr distribuciones de Linux dentro de Windows 11, sin necesidad de crear una máquina virtual con VirtualBox ni instalar dual-boot.  
En este tutorial instalaremos **Ubuntu 24.04 (Noble Numbat)**, configuraremos aplicaciones esenciales como `git`, `gedit`, y prepararemos el entorno para trabajar con **Visual Studio Code**.

## 1. WSL2 en Windows 11

Abre **PowerShell como administrador** y ejecuta:

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

Reinicia el equipo y verifica que WSL2 está activo:

```powershell
wsl --status
```
### 2. Instalar Ubuntu 24.04 LTS

1. Instala Ubuntu 24.04:
   ```powershell
   wsl --install -d Ubuntu-24.04
   ```
2. Reinicia y busca **Ubuntu 24.04 LTS** desde el menú inicio.  
3. La primera vez que se abre, te pedirá crear un usuario y contraseña Linux, pon algo simple ya que cada que corras un comando sudo te pedirá la contraseña.

### 3. Configuración inicial de Ubuntu

#### 3.1 Actualizar paquetes

Siempre lo primero que debes hacer en un Linux recién instalado es actualizar la lista de paquetes y aplicar actualizaciones. 

```bash
sudo apt update && sudo apt upgrade -y
```
Esto descargará las últimas versiones y dejará tu sistema al día.

#### 3.2 Configurar locales UTF-8

Ubuntu necesita estar configurado en un locale UTF-8 para que programas como Python, Git o ROS manejen bien los caracteres especiales.

```bash
sudo apt install -y locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
```

Comprueba que se aplicó correctamente:

```bash
locale
```
Debe mostrar `en_US.UTF-8`.

#### 3.3 Instalar programas esenciales
```bash
sudo apt install -y git curl wget build-essential cmake gedit nano vim htop unzip terminator
```

### 4. Windows Terminal (recomendado)

Instala **Windows Terminal** desde la Microsoft Store.  
Ventajas:
- Varias pestañas y splits.
- Personalización (temas, colores, atajos).
- Integra PowerShell, Ubuntu, CMD en una sola app.

Aunque puedes usar `terminator`, **Windows Terminal** es más ligero y práctico para el día a día.

### 5. Visual Studio Code con WSL

#### 5.1 Instalar VS Code
Descarga desde [code.visualstudio.com](https://code.visualstudio.com/) e instálalo en Windows.

#### 5.2 Instalar extensión *Remote - WSL*
1. Abre VS Code → pestaña **Extensions**.  
2. Busca **WSL (Microsoft)** con el ícono del pingüino 🐧.  
3. Haz clic en instalar.

#### 5.3 Conectar VS Code con Ubuntu
Da clic en el icono verde `><` en la esquina inferior izquierda y después en **Connect to WSL** y listo

### 6. Buenas prácticas en WSL

- Mantén tus proyectos en el sistema de archivos de Linux:  

- Actualiza con frecuencia:
  ```bash
  sudo apt update && sudo apt upgrade -y
  ```
- Usa `explorer.exe .` desde Ubuntu para abrir la carpeta actual en el Explorador de Windows. (Asegúrese de agregar el punto al final del comando para abrir el directorio actual.)


#### Documentación

https://learn.microsoft.com/es-mx/windows/wsl/



