# 🛡️ LLM Guard Extension

Extensión de navegador y entorno de evaluación diseñados para **detectar datos sensibles** en interacciones con modelos de lenguaje (LLMs).  
El proyecto se compone de dos módulos principales:

- **LLM_Guard_Extension/** → Código de la extensión de navegador + servidor backend para proteger la privacidad del usuario en ChatGPT.  
- **TEST/** → Experimentos de evaluación comparando 5 LLMs en la tarea de detección de información sensible.

---

## 🚀 Características principales

- **Extensión de navegador (Chromium-based)**  
  - Compatible con **Google Chrome**, **Microsoft Edge** y **Brave**.  
  - Intercepta los mensajes escritos por el usuario, los **PDF adjuntos** y las **respuestas generadas por ChatGPT**.  
  - Resalta y clasifica datos sensibles detectados en **tiempo real**.  
  - Panel flotante interactivo que muestra:  
    - Nivel de riesgo (Alto / Medio / Bajo).  
    - Campos sensibles identificados.  
    - Opciones: *Ocultar* o *Enviar de todos modos*.  

- **Servidor backend (FastAPI)**  
  - Endpoints:  
    - `/detect` → analiza texto plano.  
    - `/detect_file` → analiza PDFs (extracción de texto con `pdfplumber`, `PyPDF2`, `pdfminer`, OCR opcional).  
  - Comunicación entre extensión y modelo de detección.  
  - Respuesta en formato **JSON estructurado**.  

- **Detección de datos sensibles**  
  - Uso de un LLM (por defecto `OpenAI/gpt-oss-120B` vía API de **Groq**).  
  - Soporte para múltiples modos de análisis (*Zero-shot*, *Few-shot*…).  
  - Categorización de campos en **riesgo alto**, **medio** o **bajo**.  
  - Ejemplos: credenciales, números de tarjeta, correos electrónicos, direcciones, teléfonos, identificadores personales, etc.  

- **Experimentos con 5 LLMs**  
  - Modelos evaluados:  
    - GPT-4o-mini  
    - Gemini 1.5 Pro  
    - OpenAI/gpt-oss-120B  
    - LLaMA 4 Scout 17B 16E Instruct  
    - Claude-3 Haiku  
  - Comparación de precisión, exhaustividad y robustez en la tarea de detección de datos sensibles.  
  - Scripts y resultados disponibles en la carpeta `TEST/`.

---

## 📂 Estructura del repositorio

