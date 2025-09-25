---
title: "Transcribir audio con Whisper"
date: 2024-03-16T16:00:00+02:00
description: "Guía paso a paso para instalar OpenAI Whisper y convertir audio a texto con Python."
menu:
  sidebar:
    name: Transcribir audio con Whisper
    identifier: whisper-python
    parent: software
    weight: 1
hero: images/whisper-python-cover.jpg
tags:
- Whisper
- Python
- Transcripción
- IA
categories:
- Software
- Inteligencia artificial
---

**Whisper** es un modelo de reconocimiento de voz multilingüe de código abierto creado por OpenAI. Permite transcribir audio a texto con alta precisión sin depender de servicios en la nube.

---

## Requisitos

- Python 3.8 o superior.
- Pip y virtualenv.
- FFmpeg instalado en el sistema (necesario para convertir formatos de audio).
- GPU opcional (CUDA) para acelerar inferencias con modelos grandes.

Instala FFmpeg en Linux con `sudo apt install ffmpeg` o usa los binarios oficiales en Windows/macOS.

---

## Instalación

```bash
python -m venv .venv
source .venv/bin/activate  # En Windows: .venv\Scripts\activate
pip install --upgrade pip
pip install openai-whisper ffmpeg-python torch torchvision torchaudio
```

Si cuentas con GPU Nvidia, instala la versión de PyTorch compatible con tu versión de CUDA desde `https://pytorch.org/get-started/locally/` antes de `openai-whisper`.

---

## Transcripción básica

```python
import whisper

model = whisper.load_model("small")
result = model.transcribe("entrevista.wav", language="es")
print(result["text"])
```

El parámetro `language` acelera la detección. Usa modelos `tiny`, `base`, `small`, `medium` o `large` según recursos y precisión.

---

## Optimización y trucos

1. **Segmenta audios largos** para evitar agotar memoria (`whisper.utils.get_writer`).
2. **Activa `fp16=False`** si corres en CPU o GPUs sin soporte FP16.
3. **Guarda subtítulos** usando `writer = whisper.utils.get_writer("srt", "./salidas")`.
4. **Limpia ruido** previamente con herramientas como `pydub` o `noisereduce` para mejorar resultados.
5. **Traducción**: `model.transcribe(..., task="translate")` genera texto en inglés desde cualquier idioma.

---

## Integración en pipelines

- Automatiza carga desde S3, GCS o almacenamiento local.
- Envía resultados a bases de datos o motores de búsqueda (Elastic, Pinecone).
- Expone un microservicio FastAPI para procesar cargas de usuarios.
- Combina con diarización (Pyannote) para identificar hablantes.

Con estos pasos tendrás una solución reproducible para transcribir audio a texto usando Whisper y Python.
